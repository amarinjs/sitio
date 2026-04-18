---
title: Error detection
tags: [computer-networks]
excerpt_summary: "Parity, checksums, and CRCs — how links catch errors, and what each technique actually costs you."
---

# Intro

The chances of errors on the transmissions over the network are always present. Physical damage on a cable/NIC, Malicious corruption of data, EMI, etc.

Imagine that the packets we send and receive usually have to go through our local network, an extensive ISP maintained physical medium, trough different jumps on internet nodes, and to the server located on a remote infrastructure.


The following are some error detection (ED) algorithms used to address the above. Each of them very different from the other. And each is used on different layers of the TCP/IP protocol stack.
- Checksums
- Cyclic redundancy codes (CRCs)
- Message authentication codes (MACs)



Ethernet and TLS appends ED to the payload. 

***f(Data) = ED***


Whereas IP prepends a checksum on its header.

***ED = f(Data)***


## Checksum

It adds up all the values in the packet (IP,TCP)

<a class="button button--success button--rounded button--xs" href="">PROS</a> Fast and cheap (Even in software)

<a class="button button--primary button--rounded button--xs" href="">CONS</a> Not very robust (Weak error detection guarantees)

Easy to cheat, with as few as 2 bit errors. eg If 1 bit error adds 16 and another substracts 16, the checksum won't catch the error. 

## CRC

Computes remainder of a polynomial (Ethernet)

<a class="button button--success button--rounded button--xs" href="">PROS</a> More robust, protects against any 2 bit error

<a class="button button--primary button--rounded button--xs" href="">CONS</a> More expensive in computation terms than checksum (Easy in HW)

Because robust CRCs are used at the Link Layer, in some sense the IP layer can get away with Checksums.

eg A CRC that is ***c*** bits long, can detect any 1, 2 bit errors, any burst ***≤ c***, any odd number of errors.

## MAC

Cryptographic transformation of data (TLS)

<a class="button button--success button--rounded button--xs" href="">PROS</a> Robust to malicious modifications

<a class="button button--primary button--rounded button--xs" href="">CONS</a> Not so robust to errors

MAC is not as good as CRC

<div data-datacamp-exercise data-lang="r">
	<code data-type="pre-exercise-code">
		# This will get executed each time the exercise gets initialized
		b = 6
	</code>
	<code data-type="sample-code">
		# Create a variable a, equal to 5


		# Print out a


	</code>
	<code data-type="solution">
		# Create a variable a, equal to 5
		a <- 5

		# Print out a
		print(a)
	</code>
	<code data-type="sct">
		test_object("a")
		test_function("print")
		success_msg("Great job!")
	</code>
	<div data-type="hint">Use the assignment operator (<code><-</code>) to create the variable <code>a</code>.</div>
</div>