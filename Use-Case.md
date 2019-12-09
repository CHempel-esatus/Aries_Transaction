# Transaction Use Case

### Assumption:

#### Issuer:
  -	Employer
  -	Issued an employee credential.

#### Holder:
  -	Employee is already connected to the employer agent.
  -	Employee has the offered employee credential in his wallet.

#### Verifier:
  -	Verifier == Issuer
  -	Offering an intranet website for all employees.


### Sequence of events:
1.	Employee (holder) wants to access the intranet website of his employer (issuer and verifier).

2.	Website calls the employers SSI-Agent (verifier) to receive a QR-Code.
	1.	Website sends:
		1.	The connection ID of an already created multiparty invitation.
		2.	A proof request which describes which information the website needs to be proofed to allow access.
	2.	The QR-Code contains:
		1.	Agent Endpoint.
		2.	Query parameter t_o containing a transaction ID.
		3.	Query parameter c_i containing a multiparty connection invitation (base64 encoded).
        https://endpoint.domain?t_o=this-transaction-id&c_i=base64-encoded-connection-invitation

3.	Employee (holder) scans the offered QR-Code with his wallet app.
	1.	Wallet app does the following:
		1.	Check for t_o query parameter.
		2.	Check for c_i query parameter.
		3.	Search for the connection to endpoint + routing key from the connection invitation.
		4.	Send a TransactionResponse message to the existing connection.
			Message contains the given Transaction ID from the t_o parameter.

4.	Employers Agent (verifier) receives the TransactionResponse message.
	1.	Agent checks for the TransactionRecord for the Transaction ID from the Transaction Response Message.
		TransactionRecord contains the Proof Request from the website.
	2.	Agent sends the ProofRequest message to the connection from which it received the TransactionResponse message.

5.	Employee (holder) receives the ProofRequest message.

6.	Employee (holder) sends Proof.

7.	Employers Agent (verifier) checks for validity of the received Proof.

8.	Website grants access.
