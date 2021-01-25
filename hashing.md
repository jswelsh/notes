##bcrypt vs Argon2
***
[source](https://security.stackexchange.com/questions/193351/in-2018-what-is-the-recommended-hash-to-store-passwords-bcrypt-scrypt-argon2)

TLDR; Don't recommend bcrypt for new designs, when offline hacking is in the threat model. It lacks memory-hardness.


why choose Argon ?
- it has been around awhile now, and has withstood scrutiny
- there are open source [bindings](https://github.com/p-h-c/phc-winner-argon2) for many languages
-Argon2 is built around the AES cipher, and most modern x86_64 and ARMv8 processors implement an AES instruction set extension. This helps close the performance gap between the intended system and a dedicated cracking system. (EDIT, Feb 2019: It seems that newer versions of Argon2 may not be compatible with the AES implementation in hardware extensions. See comments for discussion around this)
- Argon2 is particularly resistant to ranking tradeoff attacks beyond a memory fraction of one third, making it much more difficult to accelerate cheaply on FPGAs. This is because FPGA-based cracking solutions are mostly limited by memory bandwidth, and with Argon2's design the attacker must swallow a great increase in computational time in order to reduce memory bandwidth requirements, thus making the tradeoff inefficient. You can read more about this in section 5.1 of the [Argon2 paper](https://github.com/P-H-C/phc-winner-argon2/blob/master/argon2-specs.pdf), with information on scrypt available for comparison in table 6 (under section 5) of the paper ["Tradeoff Cryptanalysis of Memory-Hard Functions"](https://eprint.iacr.org/2015/227.pdf).
- Memory-hardness and CPU-hardness parameters are separately configurable, along with a parallelism factor. This allows you to better tailor the security bound to your use-case, such as a server with moderate CPU power and a large amount of RAM.

As you noted, the parameters you choose are important. scrypt doesn't give you much choice, so I recommend picking a cost factor based on time (e.g. 1500ms of processing) in that case.

For Argon2 you have more choices than just parameters. There are in fact three different implementations of Argon2, known as Argon2d, Argon2i, and Argon2id. The first one, Argon2d, is most computationally expensive and resistant to acceleration by GPUs, FPGAs, and ASICs with limited memory bandwidth. However, since the memory accesses of Argon2d are dependent on the password, side-channel attacks that leak information about memory accesses may reveal the password. Argon2i, as its suffix suggests, selects memory addresses independently of the password. This reduces its resistance to GPU cracking, but eliminates the side-channel attack. Argon2id is a hybrid approach wherein the first pass uses the Argon2i (independent) approach, and subsequent passes use the Argon2d (dependent) approach.

Wherever possible you should use an Argon2id implementation. However, this is not always available. For a scenario in which you are protecting passwords on a server and your threat model deems memory access side-channel attacks to be very unlikely (this is most of the time in my experience) you may use Argon2d. If memory access side-channel attacks are deemed to be a potential risk, e.g. in a multi-tenant system where untrusted or less-trusted users run code on the same system that does the Argon2 hashing, then Argon2i may be the better choice.

In short: use Argon2id if you can, use Argon2d in almost every other case, consider Argon2i if you really do need memory side-channel attack resistance.

For the parameters, the only hard rules are for Argon2i, which must be treated specially due to its comparative weakness to the other options. Specifically, the number of iterations must be 10 or more, due to a practical tradeoff attack on Argon2i.

You should tweak the parameters to your own use-case and performance requirements, but I believe the following are acceptable defaults for Argon2id and Argon2d:

512MB of memory
8 iterations
Parallelism factor of 8
The speed of this depends on your processor, but I achieved approximately 2000ms on my system.

For Argon2i you must increase the number of iterations to a minimum of 10, which may require you to decrease the memory factor for performance reasons. This is another reason to try to avoid Argon2i unless you absolutely require its resistance to memory access side-channels.
