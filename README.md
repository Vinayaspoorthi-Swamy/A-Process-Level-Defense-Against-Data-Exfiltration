MSc-Sentinel-Lab: Behavioral Monitoring of RAG Systems
MSc Thesis Project: University of Aberdeen

A Process-Level Defense Against Indirect Prompt Injection & Data Exfiltration

Project Overview: This project implements a Zero-Trust Security Mediator designed to protect Retrieval-Augmented Generation (RAG) systems from data exfiltration. Unlike traditional AI guardrails that live inside the LLM's logic, this system operates out-of-band at the OS level.  By using a deterministic Bash-based "Circuit Breaker," the system intercepts unauthorized signals (like the ALPHA_99 token) in real-time and terminates the process using SIGKILL (Signal 9) before sensitive data can reach the user terminal. 

Key Features
Deterministic Defense: Uses Regex-based pattern matching (O(n) complexity) to ensure 100% detection of known exfiltration signatures, avoiding the probabilistic failures of "AI watching AI".  
Zero-Latency Intervention: Achieves sub-10ms detection-to-kill latency using a non-blocking tail -f I/O stream.  Process-Level Isolation: The monitor runs as an independent PID, ensuring it remains functional even if the primary Python RAG agent is fully compromised. 
Forensic Accountability: Automatically generates immutable forensic_log.json records for SIEM integration.  

System Architecture
Data Layer: ChromaDB (Vector Store) containing poisoned context.  
Processing Layer: TinyLlama (Local LLM) via LangChain. 
Defense Layer: Solitude Security Monitor (Bash Mediator).

Getting Started

Prerequisites: Ubuntu 24.04 LTS, Python 3.12, Ollama (TinyLlama), ChromaDB.  

Installation:

Bash
pip install langchain chromadb langchain-ollama
ollama pull tinyllama
sudo apt install bpfcc-tools python3-bpfcc
Execution:

Terminal 1 (The Guardian):

Bash
cd Scenario_B_Defense
chmod +x mediator_monitor.sh
./mediator_monitor.sh

Terminal 2 (The Agent):

Bash
cd Scenario_B_Defense
python3 -u rag_agent.py
The monitor will detect the adversarial ALPHA_99 token and issue an instant SIGKILL

