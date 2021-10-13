# BCrypt
## Hashing Passwords: One-Way Road to Security
* ### What's Hashing About?
    By dictionary definition, hashing refers to "chopping something into small pieces" to make it look like a "confused mess". That definition closely applies to what hashing represents in computing.

    ![encript](https://images.ctfassets.net/23aumh6u8s0i/3xP2opDfoin34ScJZMBQB5/bd706f86ade6ad72513eea23d22b039b/encryption-flow)

    - Commonly used hashing algorithms include Message Digest (MDx) algorithms, such as MD5, and Secure Hash Algorithms (SHA), such as `SHA-1` and the `SHA-2` family that includes the widely used SHA-256 algorithm.
    - Using `SHA-256`, we have transformed a random-size input into a fixed-size bit string.
    - Using `hexdigest()`, you produced a hexadecimal representation of the hash value. For any input, each message digest output in hexadecimal format has 64 hexadecimal digits. Each digit pair represent a byte. Thus, the digest has 32 bytes. Since each byte holds 8 bits of information, the hash string represent 256 bits of information in total. For this reason, this algorithm is called SHA-256 and all of its inputs have an output of equal size

* ### Cryptographic Hash Functions are Practically Irreversible
    - Hash functions behave as one-way functions by using mathematical operations that are extremely difficult and cumbersome to revert such as the modulo operator.
* ### A Small Change Has a Big Impact
    - Another virtue of a secure hash function is that its output is not easy to predict. 
    - This property is known as the avalanche effect and it has the desirable effect that if an input is changed slightly, the output is changed significantly

* ### Using Cryptographic Hashing for More Secure Password Storage
    - The irreversible mathematical properties of hashing make it a phenomenal mechanism to conceal passwords at rest and in motion. Another critical property that makes hash functions suitable for password storage is that they are deterministic.
    - A deterministic function is a function that given the same input always produces the same output. This is vital for authentication since we need to have the guarantee that a given password will always produce the same hash; otherwise, it would be impossible to consistently verify user credentials with this technique.
    - To integrate hashing in the password storage workflow, when the user is created, instead of storing the password in cleartext, we hash the password and store the username and hash pair in the database table. When the user logs in, we hash the password sent and compare it to the hash connected with the provided username. If the hashed password and the stored hash match, we have a valid login. It's important to note that we never store the cleartext password in the process, we hash it and then forget it.

    - Whereas the transmission of the password should be encrypted, the password hash doesn't need to be encrypted at rest. When properly implemented, password hashing is cryptographically secure. This implementation would involve the use of a salt to overcome the limitations of hash functions.

    - ###### Uniqueness is the key property for salts; length happens to help uniqueness.
* ### Limitations of Hash Functions
    - A brute-force attack is largely inefficient because the execution of hash functions can be configured to be rather long.
    - Since hash functions are deterministic (the same function input always results in the same hash), if a couple of users were to use the same password, their hash would be identical. If a significant amount of people are mapped to the same hash that could be an indicator that the hash represents a commonly used password and allow the attacker to significantly narrow down the number of passwords to use to break in by brute force
    
* ### No Need for Speed
    - According to Jeff Atwood, "hashes, when used for security, need to be slow." A cryptographic hash function used for password hashing needs to be slow to compute because a rapidly computed algorithm could make brute-force attacks more feasible, especially with the rapidly evolving power of modern hardware. We can achieve this by making the hash calculation slow by using a lot of internal iterations or by making the calculation memory intensive.

    - A slow cryptographic hash function hampers that process but doesn't bring it to a halt since the speed of the hash computation affects both well-intended and malicious users. It's important to achieve a good balance of speed and usability for hashing functions. A well-intended user won't have a noticeable performance impact when trying a single valid login.

## Why you should use BCrypt to hash passwords
* ### Hashed password solutions fall short
    - ##### Plain text passwords
        As its name infers, a plain text password makes use of only letters. Should a hacker gain access to passwords such as these, they can easily pose as a user on your system.
    - ##### One way hash
        With a one-way hash password, a server does not store plain text passwords to authenticate a user. Here, a password has a hashing algorithm applied to it to make it more secure. While in theory, this is a far better password solution, hackers have found ways around this system as the algorithm used is not exactly a one-way option at all. In fact, hackers can just continue to guess passwords until they gain access to your resources.
    - ##### ‘Salting’ the password
        One could consider ‘salting’ a password before it is hashed. What does this mean? Well, a ‘salt’ adds a very long string of bytes to the password. So even though a hacker might gain access to one-way hashed passwords, they should not be able to guess the ‘salt’ string. In theory, this is a great way to secure your data, but if a hacker has access to your source code, they will easily be able to find the ‘salt’ string for passwords.
    - ##### Random ‘salt’ for each user
        As an alternative, a random ‘salt’ string could be added for each user, created on the generation of the user account. This will increase encryption significantly as hackers will have to try to find a password for a single user at a time. Again, even though it means they will have to spend more time cracking the passwords for multiple users, they will still be able to gain access to your resources. It just takes longer.
* ### The BCrypt Solution
    - BCrypt is based on the Blowfish block cipher cryptomatic algorithm and takes the form of an adaptive hash function. But why should you use it to protect your data and resources? To explain, we’re going to need to get a little technical. Using a Key Factor, BCrypt is able to adjust the cost of hashing. With Key Factor changes, the hash output can be influenced. In this way, BCrypt remains extremely resistant to hacks, especially a type of password cracking called rainbow table.
    - This Key Factor will continue to be a key feature as computers become more powerful in the future. Why? Well, because it compensates for these powerful computers and slows down hashing speed significantly. Ultimately slowing down the cracking process until it’s no longer a viable strategy.
    - If you have sensitive data or information that you need to be protected, ensuring it is secured correctly is vital. As we have seen, there are many ways to secure this information through various password methods, but only BCrypt offers a truly robust solution.

* ### jBCrypt 
    is a Java™ implementation of OpenBSD's Blowfish password hashing code, as described in "A Future-Adaptable Password Scheme" by Niels Provos and David Mazières.

    - This system hashes passwords using a version of Bruce Schneier's Blowfish block cipher with modifications designed to raise the cost of off-line password cracking and frustrate fast hardware implementation. The computation cost of the algorithm is parametised, so it can be increased as computers get faster. The intent is to make a compromise of a password database less likely to result in an attacker gaining knowledge of the plaintext passwords (e.g. using John the Ripper).
    - **jBCrypt** is licensed under a ISC/BSD licence (see the LICENSE file for details) and ships with a set of JUnit unit tests to verify correct operation of the library and compatibility with the canonical C implementation of the bcrypt algorithm.