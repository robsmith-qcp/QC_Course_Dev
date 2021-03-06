# Grover
******

It is often advantageous in computing to search through a dictionary containing any number of strings, integers, etc. An example of this might be to search through a list of flight routes to find the one that will get you to your destination soonest.  In this example, if there are $N$ possible flights plans, the amount of operations needed is $O(N)$ to perform an unstructured search. However, by employing Grover's algorithm the search speeds up quadratically.  Grover's algorithm is an efficient search method that amplifies a target state through the use of an Oracle (like we saw in previous modules) and a phase shift. In this module you will guided in writing an implementation of Grover's algorithm in Qiskit.  Details about Grover's algorithm can be found in the following resource:

M. A. Nielsen and I. L. Chuang, *Quantum Computation and Quantum Information* (Cambridge University Press, USA, 2011) pp. 248-255.

---

Here is a list of integers that we will be testing our code on.

	#A list of numbers to be searched for the number 8
	secret_list=[4,3,5,6,7,8,1,2,9,3,0]

- Let's start with a Classical computing exercise where we seek a target entry in the list in an unstructured search. Begin by writing a function called c_oracle. In the function set the target to 8, and use an if-else Boolean statement to tell you if you found the target entry.

- Now, use a for loop to iterate through the list, and test each entry using the c-oracle function. Print the index the target is located in and how many calls to the oracle were performed to find this result. This is considered an unstructured search, but there are obviously more efficient ways of approaching this task.

- Now let's look at Grover's algorithm.  Grover's Algorithm amplifies the target state, making the search for it a 1-shot process.

- Import the following libraries:
		
		from qiskit import *
		from qiskit.visualization import plot_histogram
		import matplotlib.pyplot as mpl
		import numpy as np

- Let's choose the |11⟩ state as our secret state.

- Begin by building the quantum circuit that will act as our oracle. Create a 2-qubit quantum circuit and name it q_oracle. This oracle is simply a CZ gate.

- Next, build a quantum circuit consisting of two qubits and two classical bits. Apply the Hadamard gates to both qubits and then append the circuit with the q_oracle gate.

- Consider the state of both qubits. What are they at this point? Remember, our q_oracle is a CZ gate between the qubits. Run a simulation to validate your prediction.

- Now define the phase shift. A phase shift takes every computational basis state (except |0⟩) and flips the sign on the state.  To accomplish this make a 2-qubit circuit, and to both qubits apply a Hadamard, Z, CZ, and another Hadamard (acting on a ket it would look like HCZZH). Can you deduce how the application of this phase shift amplifies the signal of the target state?

- Append your circuit to include the phase shift. Measure both qubits.  Then, simulate Grover's algorithm with one shot.

- Optional: Run this circuit on a quantum computer

		from qiskit import IBMQ
		from qiskit.tools.monitor import job_monitor
		from qiskit.providers.ibmq import least_busy

		IBMQ.load_account()
		IBMQ.providers()

		provider = IBMQ.get_provider(hub='ibm-q')
		backend = least_busy(provider.backends(filters=lambda b: b.configuration().n_qubits >= 3 and
		                                   not b.configuration().simulator and b.status().operational==True))
		t_qc = transpile(grover, backend)
		job = backend.run(t_qc, shots = 1000)
		job_monitor(job) 
		result = job.result()
		counts = result.get_counts(grover)
		plot_histogram(counts)
