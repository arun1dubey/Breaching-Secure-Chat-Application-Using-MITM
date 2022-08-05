-----------------------------------------------------------------------
-----------------------------------------------------------------------
||				README				     ||
-----------------------------------------------------------------------
-----------------------------------------------------------------------

-----------------------------------------------------------------------
Team
-----------------------------------------------------------------------

Ravi Chandra Duvvuri (CS21MTECH11001)
Pratik Abhijeet Bendre (CS21MTECH12014)
Kishan Nayanbhai Bhinde (CS21MTECH11016)

-----------------------------------------------------------------------
Make Executions
-----------------------------------------------------------------------

Execute following command to send data to VM

make to_vm

Execute folowing command in VM to sends files to their respective containers

make to_con

-----------------------------------------------------------------------
Executables
-----------------------------------------------------------------------

secure_chat:
	This is a common program for both client and the server which 
execution depends on the command line arguments passed. This function 
establishes the TCP connection between them and the TLS connection on 
client discretion.

secure_chat_interceptor:
	This is the program used by the attacker to intercept the 
connection between the client and the server. This program can be 
executed in two modes; downgrade attack and Man-in-the-Middle attack 
which can be selected through command line arguments.

attacker.py:
	This program is used for ARP cache poisoning attack. The 
attacker executes the attacker code to poison the ARP cache tables of 
client and the server.

-----------------------------------------------------------------------
Task 1
-----------------------------------------------------------------------

Alice and Bob containers contains the certificate signing requests, 
certificates, key pairs in the respective file systems and CA 
certificate is uploaded in the root store of both containers which are 
generated using OpenSSL commands.

-----------------------------------------------------------------------
Task 2
-----------------------------------------------------------------------

1. On Bob's container execute secure_chat.py with '-s' command line 
argument to execute server code.

		./secure_chat -s

2. On Alice's container execute secure_chat.py with '-c' command line 
argument and the hostname of the server to execute client code.

		./secure_chat -c bob1

3. Capture the pcap using the following command:
		tcpdump -i eth0 -w <pcap_name>.pcap
		
NOTE: To move to the container of host, use the following command:
		lxc exec host bash

The TCP connection will be established between Alice and Bob, and it
is to the discretion of Alice to start a TLS connection.

-----------------------------------------------------------------------
Task 3
-----------------------------------------------------------------------
1.To start with this attack, we need to poison the dns using

		./poison-dns-alice1-bob1.sh


2. On Bob's container execute secure_chat.py with '-s' command line 
argument to execute server code.

		./secure_chat -s
		
3. On Trudy's container execute secure_chat_interceptor.py with '-d' and
the hostnames of client and server

		./secure_chat_interceptor -d alice1 bob1

4. On Alice's container execute secure_chat.py with '-c' command line 
argument and the hostname of the server to execute client code.

		./secure_chat -c bob1

5. Capture the pcap using the following command:
		tcpdump -i eth0 -w <pcap_name>.pcap
-----------------------------------------------------------------------
Task 4
-----------------------------------------------------------------------
1.To start with this attack, we need to poison the dns using

		./poison-dns-alice1-bob1.sh


2. On Bob's container execute secure_chat.py with '-s' command line 
argument to execute server code.

		./secure_chat -s
		
3. On Trudy's container execute secure_chat_interceptor.py with '-m' and
the hostnames of client and server

		./secure_chat_interceptor -m alice1 bob1

4. On Alice's container execute secure_chat.py with '-c' command line 
argument and the hostname of the server to execute client code.

		./secure_chat -c bob1

5. Capture the pcap using the following command:
		tcpdump -i eth0 -w <pcap_name>.pcap
-----------------------------------------------------------------------
Bonus
-----------------------------------------------------------------------

1. On Trudy's container execute attacker.py to poison the ARP caches 
of Alice and Bob.

		python3 attacker.py

2. Execute the secure_chat.py in Bob's container in server mode using '-s'
 command line argument.

		./secure_chat -s

3. Execute the secure_chat.py in Alice's container in client mode using 
'-c' command line argument and hostname of the server, here bob1.

		./secure_chat -c bob1

4. Execute secure_chat_interceptor.py in Trudy's container in server mode 
to intercept the communication between Alice and Bob.

		./secure_chat_interceptor -m alice1 bob1
