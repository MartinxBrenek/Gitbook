---
layout:
  width: default
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
  metadata:
    visible: true
  tags:
    visible: true
  actions:
    visible: true
---

# AI and ML

### Core definitions

**Artificial Inteligence (AI)** is an umbrella that can be explained like a broad term encompassing various technologies and machines, capable of performing tasks that typically require human-like intelligence, such as problem-solving, reasoning, discovering meaning, and recognizing patterns.

**Machine Learning (ML)** A subset of AI describes mathematical and statistical methods that enable machines to mimic intelligent human behavior by learning from data without being explicitly programmed.

**Deep Learning (DL)** subfield of machine learning that focuses on building artificial neural networks with multiple layers to solve complex problems. Ability to automatically learn representations of the data, without the need for manual feature engineering

Classical programming and scripting have been used for a long time to automate and manage various networking tasks. These traditional methods excel at handling straightforward, rule-based tasks, such as configuring network devices, monitoring performance, and managing network policies. For example, a script can be written to automatically update the firmware on a set of routers or to periodically check the status of network interfaces.

Many complex tasks are challenging or even impossible to solve with classical programming. These tasks often involve analyzing vast amounts of data, identifying patterns, predicting future network states, or making real-time decisions in dynamic environments. One such example is the detection of network anomalies and security threats. Although a script can be programmed to recognize known attack patterns, it struggles to identify new, evolving threats or subtle anomalies that do not match predefined rules.

To address these challenges, machine learning (ML) and artificial intelligence (AI) techniques are introduced into network operations.

ML and AI can analyze large datasets to learn what is considered normal network behavior (baselining), detect anomalies, predict failures, and suggest optimal configurations. For instance, an AI-powered system can continuously monitor network traffic and identify unusual patterns that are indicative of a potential security breach, even if those patterns are new.

AI and ML are intricate fields that require extensive specialized knowledge in computer science and data analytics. Solving complex problems requires dedicated teams of experts who integrate these advanced tools into various products. Leading companies, including Cisco, have embraced this approach.

In network operations, ML is used to create AI systems that can monitor, manage, and optimize network performance. For example, ML algorithms can analyze vast amounts of network traffic data to identify patterns and anomalies. By running these algorithms on data collected from past events, the AI system can learn to predict potential issues, such as network congestion or failures before they occur. The process of using data and ML techniques to teach an AI system how to perform a specific task is called training. After AI is trained, it can autonomously adjust network configurations in real time to prevent or mitigate potential issues, which ensures optimal performance and reduces downtime.

**Predictive AI** involves the use of advanced algorithms and statistical models to analyze past and current data to forecast future events or behaviors. This type of AI uses ML techniques to identify patterns and trends within the data, which enables it to make informed predictions about what is likely to happen next. In various industries, predictive AI is employed to anticipate customer behavior, optimize inventory management, predict equipment failures, and more.

In the context of network operations, predictive AI is used in proactive network maintenance and network performance optimizations. By analyzing past network traffic, performance metrics, and incident reports, predictive AI can see potential issues, such as network congestion, hardware failures, and security breaches. This enables network administrators to take preventive actions, optimize resource allocation, and ensure smoother, more reliable network operations.

**Generative AI** involves creating new content or data that is based on patterns learned from existing datasets. Using advanced models, such as generative adversarial networks (GANs), variational autoencoders (VAEs), and generative pre-trained transformers (GPT), generative AI can produce output that closely mimics the characteristics of the training data. For now, keep these terms in mind, you will learn about these models later. This technology can generate diverse outputs, which includes text, images, audio, and synthetic data for simulations.

In network operations, generative AI plays a crucial role. It can simulate network scenarios by generating realistic network traffic patterns. These simulations enable testing and evaluating network performance under various conditions without affecting the live network. Also, generative AI can automatically create optimal network configurations and settings based on current demands and predictive insights. Generative AI also helps in developing potential solutions for identified network issues, which provides multiple options for remediation and optimization.

Generative and predictive AI are often used together in network operations. Predictive AI identifies and anticipates potential network problems, while generative AI creates simulations and solutions to address these issues proactively. By combining the strengths of both AI types, network administrators can ensure more resilient, efficient, and dynamic network operations.

The rise of **Large Language Models (LLMs)** has significantly popularized the use of AI by providing a more human-like way of interacting with the AI technology. LLMs are generative AI systems that are designed to engage in human-like conversations. Before the introduction of such models, interacting with AI systems often required specialized knowledge, limiting their use to experts or those with backgrounds in AI and ML.

LLMs are trained using sophisticated algorithms, such as Generative Pre-trained Transformers (GPT) and on extensive datasets. LLMs can also be coupled with algorithms, such as Generative Adversarial Networks (GANs) or Variational Autoencoders (VAEs) to generate images based on text, which is known as multi-modality. These advanced algorithms are based on neural networks, inspired by the neurons in the human brain.

### Neural networks (the building block)

A neural network consists of a series of interconnected nodes, often referred to as neurons. These nodes are typically organized into layers, as shown in the following figure.

![](<../.gitbook/assets/Unknown image (45)>)

Within this structure, every node in one layer is connected to every node in the next layer, facilitating the flow of information from the input layer, through one or more "hidden" layers, to the output layer. This architecture allows the neural network to process and transform input data step by step until it reaches the final output.

In a neural network, each node holds a numerical value. The connections between the nodes are associated with weights, which are numerical values that influence how much one node affects the other. These weights determine the strength and direction of the influence that a node has on the nodes in the subsequent layer to which it is connected. To calculate the value of the first node in a hidden layer, you take each input from the preceding layer and multiply it by the corresponding weight that connects it to this node. Then, you sum up all these products. This summed value represents the initial input to the node before any further processing. Each node, apart from the input layer, has an additional parameter, which is called a bias, that is added to the initial summed value, which helps the neural network to learn more patterns. For instance, without bias, a neuron's output would always be zero whenever all its inputs are zero.

The real computational work happens within the "hidden layers," which you can think of as a series of adjustable knobs, that fine-tune the network’s ability to solve problems.

![](<../.gitbook/assets/Unknown image (46)>)

During training, the network receives input data at the input layer. After the data has moved through the hidden layers, the network produces output at the output layer. The network's output is compared to the actual expected result, and the difference between them is calculated. This difference is known as the error and evaluates how well the network performed. To improve the performance of the neural network, a technique that is called backpropagation is used. During this step, the weights and biases are recalculated so the error between the real and expected output is minimized. The entire process is repeated numerous times until the error is small enough.

![](<../.gitbook/assets/Unknown image (47)>)

To clarify, imagine you want to train a neural network to recognize whether a provided IP is valid or not. Assume four nodes at the input layer, one for each octet, a single hidden layer, and one output node that gives a 1 if the IP is valid and 0 otherwise.

At first, the weights and biases of the hidden layer are randomly defined. Assume you check whether 10.10.20.1 is valid, and the output returns a 0, since the neural network estimated that this was not the valid IP.

Now you use backpropagation to recalculate the weights and biases, so that you get a value of 1 at the output. You repeat this process until you get the expected results for any IP. Note, however, that this is a very simplified, high-level overview of how a neural network works. Also note that there are different kinds of neural networks that are used for specific tasks.

![](<../.gitbook/assets/Unknown image (48)>)

### Example applications in networking

#### Anomaly detection (IDS)

Neural networks can learn patterns of normal network traffic.

They detect deviations (anomalies) that may indicate attacks (DoS, port scans, malware)

Example: Given packet size, IP headers, protocol types, etc., the network flags suspicious activity.

#### Traffic prediction and load balancing

Predict future bandwidth usage or congestion using historical data.

Helps in dynamic routing, resource allocation, and reducing latency.

Example: A trained model predicts high usage at 6 PM daily, so routes are pre-adjusted.

![](<../.gitbook/assets/Unknown image (49)>)

![](<../.gitbook/assets/Unknown image (50)>)

![](<../.gitbook/assets/Unknown image (51)>)

GPT models demonstrated remarkable capabilities in understanding and generating text. However, they sometimes struggle to provide accurate responses and may even offer factually incorrect answers. These models are probabilistic by nature, meaning they generate the most likely response, which is not necessarily the most accurate. In other words, GPT models will always provide an answer, even if it is incorrect. These models are also constrained by their initial training set and may produce incorrect responses when tasked with generating context-specific text that was not included in their initial training data.

A much more cost-effective method for enhancing the knowledge of GPT models involves fine-tuning. In this approach, you take a pre-trained GPT model and train it using a smaller dataset that is tailored to your specific needs. For example, you could reformat the entire CCNA course to serve as a training dataset. During fine-tuning, only a small subset of the model's parameters, such as the weights and biases of the neural networks, are modified. However, altering the parameters of a pre-trained model carries risks, as the model can forget previously learned information, a phenomenon known as "catastrophic forgetting." Although less expensive than training from scratch, fine-tuning still demands considerable expertise to prevent catastrophic forgetting.

There are several ways that you can add topic-specific knowledge to a GPT model. The most effective, but also the most time-consuming and expensive is training a GPT model from scratch. To properly train such a model, you would need enormous amounts of training data and a very large and performant GPU cluster. For example, ChatGPT-3 training was estimated to require around 1000 high-end GPUs and about 34 days of training, costing around $4M.

Both training and fine-tuning are unsuitable for LLMs that require frequent updates with the latest information, due to the inherently high demands of both processes.

Finally, RAG was designed as a cost-effective and risk-free method to provide topic-specific context to a query, vastly improving accuracy of the responses that are grounded in factual data. This method is based on external datasources, used as a knowledge base, and does not change any of the model parameters. The low cost and the ability to simply add or update relevant information made RAG very popular in the AI community.

### Retrieval-Augmented Generation (RAG)

**RAG** integrates the content of the files that you provided with the query—prompt.

The content from the files provides real, factual information, which is processed together with the query by the Large Language Model (LLM), as shown in the figure.

![](<../.gitbook/assets/Unknown image (52)>)

This is very similar as writing the required context into a prompt together with a query. For example, you could paste the output of a "show running-config" command into the prompt and include a query, such as: "Is SSH enabled?" The LLM will find the relevant information in the prompt itself and provide an accurate answer. You can only imagine what would happen if you forgot to paste the output of the command. The described approach is good enough for shorter contexts and simple queries. For longer contexts and complicated queries, the context-in-prompt method becomes increasingly impractical and even impossible to implement due to the length limitations of the prompt itself.

RAG uses an external database to store the contexts and retrieves relevant information from the database on the fly when a query is passed into the system. In our example, the output of the "show running-config" command would first be stored into the database. When you prompt the RAG system, it uses the query to find relevant information in the database, in this case, it would look for anything Secure Socket Shell (SSH) related and extract this information from the database. Next, the retrieved configuration regarding SSH and the prompt are sent to the LLM for processing, which provides an accurate response.

![](<../.gitbook/assets/Unknown image (53)>)

Preprocessing: The content that is used for context can come from various sources, such as ordinary files, databases, or even live feeds. The RAG system extracts the text content from the various sources and splits it into smaller, more manageable pieces called tokens. A token can be a word, a phrase, or even only a part of a word, depending on the tokenizer used. The tokens are then grouped into meaningful chunks, such as noun or verb phrases. For example, the line "transport input ssh" from the output of the "show running-config" command might be split into the following tokens: \["transport", "in", "put", "SSH"]. These tokens might be grouped into the following chunks: \["transport input", "SSH"]. The grouping into chunks is especially useful in tasks that benefit from understanding higher-order structures within the text. The splitting of text is an extremely important part of the RAG process, since it significantly impacts the quality and efficiency of context retrieval. The process ensures that the RAG system can handle and process large quantities of text efficiently.

Embedding: Once the text is processed, or consumed in the jargon of RAG, it has to be transformed in a format optimized for search and retrieval—a process called embedding. During this phase, the chunks are passed to a specialized LLM called an embedding model, which converts them into multidimensional vectors (arrays of numbers). These vectors function similarly to geographical coordinates on a map, with each point symbolizing the underlying meaning of the text. Essentially, the numerical values within these vectors capture the essence of what the text conveys. Just as points that are close together on a map represent locations that are near each other, vectors that are close in distance indicate texts that share similar meanings or themes. The vectors are then stored in a specialized vector database, which functions like a dictionary where the lookup key is the vector, and the associated value is the original text. This step ensures that the semantic meaning of the text is preserved and can be efficiently accessed.

Retrieval: When you enter a query into the RAG system, it is also converted into vectors by the same process as the text from the files. The system calculates the "distances" between these vectors and the stored vectors to quickly determine which texts are closest to the query, indicating their relevance. The system then gathers the most relevant texts based on the closest vectors, using them as context for response generation. This method, which is known as semantic search, ensures that the most contextually relevant information is retrieved. For example, the query "Is SSH enabled?" is semantically similar to "ip ssh version 2" and "transport input ssh" lines from the "show running-config" command output due to the presence of the word SSH. The two lines would be collected as context for the example query.

Generation: Both the prompt and the context are passed to the general LLM in their original text format, allowing the LLM to produce a factually correct and contextually accurate response.

#### RAG in network operations

One of the primary applications of RAG in network operations is the automation of troubleshooting and diagnostics. When a network issue arises, a RAG system can quickly analyze the query describing the problem and retrieve relevant historical data, previous incident reports, and documented solutions from a vast database. By accessing this collective knowledge, the system can generate insightful, contextually relevant suggestions or steps to resolve the issue, significantly speeding up the troubleshooting process.

Another potential use is in predictive maintenance. RAG systems can process descriptions or logs of network behavior to forecast potential failures or identify parts of the network that might require attention. By combining past data with real-time inputs, these systems can suggest preemptive actions that prevent downtime.

RAG can handle user queries in real time, providing technical support, or guiding network configuration changes. For instance, if a network engineer asks how to configure a specific device or troubleshoot an error, the RAG system can generate precise, step-by-step configuration instructions or troubleshooting guides by retrieving similar cases and their resolutions.

RAG can also facilitate the documentation process. As network configurations change or new issues are resolved, the system can automatically generate updated documentation that accurately reflects the latest network state and troubleshooting procedures. This not only saves time but also ensures that the documentation is comprehensive and up-to-date.

### Application of AI and ML in network operations

AI and ML are transforming network operations by enhancing efficiency, reliability, and security, shifting from reactive to proactive management. This transformation has significantly benefited several key areas

Automated Configuration and Management of Network Settings: AI and ML facilitate automated configuration and management of network settings, streamlining routine tasks and significantly reducing the likelihood of human error. These intelligent systems can autonomously configure network devices, apply updates, and adjust settings in real-time based on evolving network demands and conditions.

Traffic Analytics and Management: By examining traffic patterns and predicting future congestion points, AI-driven solutions can dynamically adjust routing protocols and allocate bandwidth more efficiently. This ensures smooth network operation, maintaining high performance and quality of service for users.

Anomaly Detection and Security: AI models continuously monitor network traffic in real-time, identifying deviations that could indicate security threats such as distributed denial of services (DDoS) attacks, malware, or unauthorized access attempts. By understanding normal traffic patterns, these models can swiftly detect and alert administrators to suspicious activities, allowing for quick responses to mitigate potential threats.

Predictive Maintenance: AI algorithms analyze data from network equipment to forecast potential failures before they happen. This proactive approach not only minimizes downtime but also extends the lifespan of network hardware by enabling timely preventive maintenance.

Root Cause Analysis: AI aids in root cause analysis, a traditionally time-consuming process. By analyzing data across various network elements, AI can quickly pinpoint potential problems, accelerating troubleshooting and reducing network downtime.

### Cisco AIOps in network operations

AIOps, short for Artificial Intelligence for IT Operations, refers to the application of artificial intelligence and machine learning techniques to enhance and automate IT operations. Cisco has developed a suite of products that stand at the forefront of the AIOps revolution in network engineering.

These solutions are crafted to make networks more autonomous and efficient, with the ability to self-optimize and quickly resolve issues.

![](<../.gitbook/assets/Unknown image (54)>)

Cisco Catalyst Center (previously known as Cisco DNA Center) is central to Cisco intent-based networking. It offers centralized management, automation, and orchestration across the entire network. Within Cisco Catalyst Center, the AI Network Analytics feature provides a robust array of capabilities, including intelligent issue detection through AI-driven baselining and anomaly detection, proactive insights for trend and pattern identification, and comparative benchmarking to evaluate network performance against peers or other sites. By continuously collecting and analyzing network data, Cisco AI Network Analytics adapts to evolving network conditions, enabling IT teams to proactively address potential issues and improve overall network performance.

In the figure, you can see the average Client RSSI (Received Signal Strength Indicator) over a week for two buildings in San Francisco and San Jose, grouped into three distinct categories (Low, Medium High). The plot was produced by Cisco AI Network Analytics engine and showcases how AI helps engineers to assess average signal strength received by clients in different locations.

![](<../.gitbook/assets/Unknown image (55)>)

Cisco Meraki advanced WAN analytics use ML algorithms to enhance management, troubleshooting, and optimizing connectivity and uptime. AI within Cisco Meraki enhances security through real-time threat detection and microsegmentation and powers smart cameras and environmental sensors for monitoring safety and compliance. AI and ML also provide operational insights, facilitating rapid anomaly response and process optimization.

The following figure shows one of the Meraki AI powered features—the Auto RF solution that automatically optimizes the wireless radio parameters, such as selecting the best available channel and adjusting the power level, allowing you to maximize performance and minimize interference.

Cisco Nexus Dashboard provides a unified management and monitoring pane for data center networks, hosting applications like Cisco Nexus Dashboard Insights (NDI). AI and ML in Cisco NDI are used for Event Analytics to refine control-plane event analysis. These technologies detect correlations between configuration changes, control-plane faults, and events, identifying anomalies that could disrupt network operations.

Cisco AppDynamics is a comprehensive application performance management and IT operations analytics platform. It uses advanced AI and ML technologies to enhance the observability and operational efficiency of both applications and infrastructure. Using machine learning, AppDynamics automatically detects performance issues by establishing dynamic baselines that account for historical data, including time-of-day and seasonal variations, facilitating immediate anomaly detection. It also uses ML-driven root cause analysis to pinpoint the underlying causes of anomalies and integrates log analysis tools to identify outliers and detect log patterns. These AI and ML capabilities ensure optimal application performance by quickly identifying and addressing issues.

The following figure shows a drill-down of a Transaction snapshot in Cisco AppDynamics where Machine Learning algorithms in the background work on root cause analysis and help you pinpoint and resolve issues.

![](<../.gitbook/assets/Unknown image (56)>)

Cisco ThousandEyes is a network intelligence platform that offers visibility into the digital delivery of applications and services over the internet. It helps organizations monitor, troubleshoot, and optimize their network infrastructures, including traditional networks, cloud networks, and SaaS applications. Using AI and ML technologies, Cisco ThousandEyes automatically analyzes historical and current data to detect anomalies and disruptions within the network infrastructure, enhancing the ability to maintain and improve network performance.

Cisco Secure Network Analytics (formerly Stealthwatch) is a security product that uses machine learning to monitor network traffic in real-time, quickly identifying and responding to anomalies that could indicate security threats. By analyzing historical network traffic data that is categorized as normal or malicious, the system learns to recognize patterns and signatures that are associated with various types of cyber threats, such as malware, ransomware, DDoS attacks, and unauthorized access attempts. When it detects traffic that matches these threat characteristics, it alerts network administrators to take appropriate action. This ability to distinguish between benign and potentially harmful network activity enables organizations to proactively defend their networks against both known and emerging threats.

### Vibe coding

**Vibe coding** is an approach to producing software by using artificial intelligence (AI), where a person describes a problem in a few natural language sentences as a prompt to a large language model (LLM) tuned for coding. The LLM generates software based on the description, shifting the programmer's role from manual coding to guiding, testing, and refining the AI-generated source code.\[1]\[2]\[3]

Advocates of vibe coding say that it allows even amateur programmers to produce software without the extensive training and skills required for software engineering.\[4] Critics point out a lack of accountability and increased risk of introducing security vulnerabilities in the resulting software. The term was introduced by Andrej Karpathy in February 2025\[2]\[4]\[1] and listed in the Merriam-Webster Dictionary the following month as a "slang & trending" term.\[5]

Definition

Computer scientist Andrej Karpathy, a co-founder of OpenAI and former AI leader at Tesla, introduced the term vibe coding in February 2025. The concept refers to a coding approach that relies on LLMs, allowing programmers to generate working code by providing natural language descriptions rather than manually writing it.

### AI agents

An **AI agent** is a software entity that acts autonomously to achieve specific goals. It can perceive its environment, reason, and make decisions. There are various types:

Types of AI Agents:

Reactive Agents: Respond to stimuli without internal memory (e.g., basic bots).

Deliberative Agents: Use internal models and planning.

Learning Agents: Improve performance over time using data (e.g., reinforcement learning).

Multi-Agent Systems: Multiple agents interacting in a shared environment.

Real-World Examples:

ChatGPT (as an assistant agent)

Autonomous vehicles (navigation agents)

Game bots (NPC behavior agents)

Customer support bots

AI-based trading bots

Key Traits:

Autonomy: Operate without direct human intervention.

Perception: Take input from the environment.

Action: Influence the environment.

Learning/Reasoning: Adapt behavior based on data or rules.

### AI infrastructure (networking implications)

The infrastructure for AI uses the same routing/switching protocols and techniques to handle the vast amount of traffic trasnmitted between AI servers.

The QoS techniques will be more prevelant and used in such environments due to the bursty nature of the AI infrastructure.

#### RoCEv2 (RDMA over Converged Ethernet v2)

**RoCEv2 (RDMA over Converged Ethernet v2)** is a network protocol that enables Remote Direct Memory Access (RDMA) over an Ethernet network, allowing low-latency, high-bandwidth data transfers between devices. It builds on the original RoCE by adding routability, operating at the internet layer using UDP/IP (port 4791), which makes it suitable for larger, multi-layer networks like data centers. RoCEv2 bypasses the host CPU and OS network stack, directly accessing memory via RDMA-enabled network cards, reducing latency and CPU overhead. It includes features like congestion control (using ECN and CNP frames) and supports Quality of Service (QoS), making it ideal for high-performance computing, storage, and AI workloads. Unlike its predecessor, RoCEv2 packets are routable across IP networks, enhancing its scalability for hyperscale data centers

[https://www.cisco.com/c/en/us/td/docs/unified\_computing/Intersight/IMM-RoCE-Configuration-Guide/b-imm-rdma-over-converged-ethernet--roce--v2/m-rdma-over-converged-ethernet--roce--version-2.html](https://www.cisco.com/c/en/us/td/docs/unified_computing/Intersight/IMM-RoCE-Configuration-Guide/b-imm-rdma-over-converged-ethernet--roce--v2/m-rdma-over-converged-ethernet--roce--version-2.html)

![](<../.gitbook/assets/Unknown image (57)>)

#### InfiniBand (IB)

**InfiniBand (IB)** alternative to Ethernet and Fibre Channel used in high-performance computing (HPC) environments

IB provides high bandwidth and low latency. IB can transfer data directly to and from a storage device on one machine to userspace on another machine, bypassing and avoiding the overhead of a system call. IB adapters can handle the networking protocols, unlike Ethernet networking protocols which are ran on the CPU. This allows the OS's and CPU's to remain free of load while the high bandwidth transfers take place. IB hardware is made by Mellanox (nVIDIA)

![](<../.gitbook/assets/Unknown image (58)>)

NVMe oF (NVMe over Fabrics) is a powerful technology for enterprises that enables fast data transfer between enterprise systems and solid-state drives.

NVMe is an interface specification and storage protocol, while NVMe oF facilitates communication over a network (fabric) like Ethernet, fiber channel, or InfiniBand.

NVMe oF uses a message-based model for communication between a host and a storage device, allowing communication over longer distances compared to local NVMe.

NVMe oF is an attractive alternative to iSCSI, offering lower latency and providing data centers with unmatched access to NVMe SSD storage.

With NVMe oF, data transfer across the network takes just microseconds, enhancing overall performance.

iSCSI stands for Internet Small Computer System Interface. It is a network protocol that allows the transmission of SCSI (Small Computer System Interface) commands over an IP network. It enables the use of IP networks, such as Ethernet, to connect and access storage devices remotely. iSCSI allows servers and computers to treat remote storage devices as if they were directly attached to the local system, providing block-level access to storage resources. By using iSCSI, storage systems can be centralized and shared across a network, simplifying storage management and enabling storage consolidation.

### What is the Model Context Protocol? MCP defined <a href="#what-is-the-model-context-protocol-mcp-defined" id="what-is-the-model-context-protocol-mcp-defined"></a>

The Model Context Protocol (MCP) is an [open source](https://www.infoworld.com/article/2262355/what-is-open-source-software-open-source-and-foss-explained.html) framework that aims to provide a standard way for AI systems, like [large language models](https://www.infoworld.com/article/2335213/large-language-models-the-foundations-of-generative-ai.html) (LLMs), to interact with other tools, computing services, and sources of data. Helping [generative AI](https://www.infoworld.com/article/2338115/what-is-generative-ai-artificial-intelligence-that-creates.html) tools and [AI agents](https://www.computerworld.com/article/3843138/agentic-ai-ongoing-coverage-of-its-impact-on-the-enterprise.html) interact with the world outside themselves on their own is a key to allowing autonomous AI to take on real-world tasks. But it has been a difficult goal for AI developers to realize at scale, with much effort being put into complex, bespoke code that connects AI systems to databases, file systems, and other tools.

[Several protocols have emerged](https://www.infoworld.com/article/4007686/a-developers-guide-to-ai-protocols-mcp-a2a-and-acp.html) recently that aim to solve this problem. MCP in particular has been adopted at an increasingly brisk pace since it was [introduced by Anthropic in November 2024](https://www.infoworld.com/article/3613143/anthropic-introduces-the-model-context-protocol.html). With MCP, intermediary client and server programs handle the communication between AI systems and external tools or data, providing a standardized messaging format and set of interfaces that developers use for integration. In this article, we’ll introduce you to the Model Context Protocol and talk about its impact on the world of AI.

### What is an MCP server? <a href="#what-is-an-mcp-server" id="what-is-an-mcp-server"></a>

Before diving into the details of the different building blocks of MCP architecture, we first need to define the _MCP server,_ since it has become nearly synonymous with the protocol itself. An MCP server is a lightweight program that sits between an AI system and some other service or data source. The server acts as a bridge, communicating with the AI (via an MCP client) in a standardized format defined by the Model Context Protocol, and with the other service or data source via whatever programmatic interface it exposes.

MCP servers are relatively simple to build. A wide variety of them, which can do anything from [interact with a database](https://github.com/FreePeak/db-mcp-server) to [get the latest weather reports](https://github.com/adhikasp/mcp-weather), are [available on GitHub](https://www.infoworld.com/article/4006787/github-launches-remote-mcp-server-in-public-preview-to-power-ai-driven-developer-workflows.html) and elsewhere. The vast majority are free to download and use (though paid MCP servers also are emerging). This is part of what has made MCP so popular so quickly: Developers and users of all sorts of AI systems found that they could use these readily available MCP servers for a variety of tasks. And that widespread adoption was made possible by the way that MCP servers connect to the AI tools themselves.

### MCP vs. RAG vs. function calling <a href="#mcp-vs-rag-vs-function-calling" id="mcp-vs-rag-vs-function-calling"></a>

MCP isn’t the first technique developed to connect AIs to the outside world. For instance, if you want an LLM to integrate documents that aren’t part of its training data into its responses, you can use [retrieval augmented generation (RAG)](https://www.infoworld.com/article/2335814/what-is-retrieval-augmented-generation-more-accurate-and-reliable-llms.html), though that involves encoding the target data into a vector-formatted database.

Many LLMs are also capable of what’s variously known as _function calling_ or _tool use._ The LLMs can recognize when a user prompt or other input is requesting functionality that requires the use of a helper tool, and instead of a natural language response, it would instead provide the appropriate commands to invoke that tool. So, for instance, if you asked a general-purpose chatbot about the climate in Los Angeles, it could almost certainly give you all that information from its training data; but if you asked it what the weather in Los Angeles was going to be tomorrow, it would need to connect to a service that provides weather forecasting data.

This connection task was far from impossible: After all, LLMs are at the heart text generators, and the commands these services expect take the form of text. But building out the tools necessary to connect LLMs to outside services turned out to be a daunting task, and many developers found themselves reinventing the wheel each time they had to code a connector.

“If you were doing tool calling a year ago, every agentic framework had its own tool definition,” says [Roy Derks](https://www.linkedin.com/in/gethackteam/), Principal Product Manager, Developer Experience and Agents at IBM. “So if you switched frameworks, you’d have to redo your tools. And since tools are just mappings to APIs, databases, or whatever, it was hard to share them.”

MCP builds on LLMs’ capability to make function calls, providing a much more standardized way to connect to services and data sources. “The LLM is not aware of MCP,” Derks says. “It only sees a list of tools. Your agentic loop, however, knows the mapping of the tool name to MCP, so it knows MCP is my mode of calling tools. If my get-weather tool is being called or suggested by the large language model, it knows it should call the get-weather tool and then—if this happens to be a tool from an MCP server—it uses the MCP client-server connection to make that tool call.”

### MCP architecture: How MCP works <a href="#mcp-architecture-how-mcp-works" id="mcp-architecture-how-mcp-works"></a>

Now we’re ready to take look in more detail at the different components that make up the MCP architecture and how they work together.

### InfoWorld Smart Answers

&#x20;[Learn more](https://www.infoworld.com/smart-answers)

**Explore related questions**

* [How quickly can developers integrate MCP into their existing applications?](https://www.infoworld.com/article/4029634/what-is-model-context-protocol-how-mcp-bridges-ai-and-external-services.html)
* [Why does the Model Context Protocol increase the risk of shadow AI?](https://www.infoworld.com/article/4029634/what-is-model-context-protocol-how-mcp-bridges-ai-and-external-services.html)
* [Can MCP vulnerabilities allow hackers to compromise my developer laptop?](https://www.infoworld.com/article/4029634/what-is-model-context-protocol-how-mcp-bridges-ai-and-external-services.html)
* [Can I use MCP to manage my cloud resources with natural language?](https://www.infoworld.com/article/4029634/what-is-model-context-protocol-how-mcp-bridges-ai-and-external-services.html)
* [How can DevOps teams use MCP to automate complex backend workflows?](https://www.infoworld.com/article/4029634/what-is-model-context-protocol-how-mcp-bridges-ai-and-external-services.html)

Ask

* An **MCP host** is the AI-based application that will be connecting to the outside world. When Anthropic rolled out MCP, the company built it into the Claude desktop application, which became one of the first MCP hosts. But hosts aren’t limited to LLM chatbots—an AI-enhanced IDE could serve as a host as well, for instance. A host program includes the core LLM along with a variety of helper programs.
* An **MCP client** is, for our purposes, the most important of these helpers. Each LLM needs a client that has been customized to accommodate the way it calls tools and consumes data. However, all MCP clients provide a standard set of services to their hosts. They discover accessible servers and report back on those services and the parameters required to call them, all of which information goes into the LLM’s prompt context. When the LLM recognizes user input that should trigger a call to an available service, it uses the client to send out a request for that service to the appropriate MCP server.
* We’ve already discussed the **MCP server**, but now you have a better sense of how it fits into the bigger picture. Each server is built to communicate with a data source or external service in a language that data source or service understands, and communicates with the MCP client in accordance with the Model Context Protocol, serving as a middleman between the two.
* The MCP client and MCP server communicate with one another using a JSON-based format. The **MCP transport layer** converts MCP protocol messages into JSON-RPC format for transmission and converts JSON-RPC messages back into MCP protocol messages on the receiving end. Note that a server might run locally on the same machine as the client, or might run online and accept client connections over the internet. In the former scenario client-server communications take place via stdio, and in the latter via streamable HTTP.

The figure below illustrates how these components all work together. As noted, servers can run either locally or remotely to clients, and servers can connect to either local or remote services.

<figure><img src="https://b2b-contenthub.com/wp-content/uploads/2025/07/How-Model-Context-Protocol-works.jpg?quality=50&#x26;strip=all&#x26;w=1024" alt="Diagram showing how Model Context Protocol (MCP) functions" height="576" width="1024"><figcaption></figcaption></figure>

[Foundry](https://modelcontextprotocol.io/docs/getting-started/intro)

### MCP server ecosystem <a href="#mcp-server-ecosystem" id="mcp-server-ecosystem"></a>

The increasing popularity of MCP is largely due to the wide variety of servers available free of charge to anyone who wants to use them. And while having all these components in the architecture might at first seem more complex than simply connecting an LLM directly to an outside service, MCP’s modularity, portability, and standardization make everything much easier for developers:

* An MCP server can communicate with any AI application that includes a properly implemented MCP client. That means that if you want to expose your service for access by AI agents, you can write a single server and know it will work across different types of LLMs.
* Conversely, while an MCP client must be tailored to a specific host, it can connect to any properly implemented MCP server. There is no need to figure out how to connect your specific LLM to Google Docs, or to a MySQL database, or to a weather forecasting service. You just need to create an MCP client and it can, via MCP servers, connect to services of all types.

To take a deep dive into the ecosystem of MCP servers, check out the [Model Context Protocol servers GitHub repo](https://github.com/modelcontextprotocol/servers).

### MCP security issues <a href="#mcp-security-issues" id="mcp-security-issues"></a>

Just about any method that opens up new lines of communication also provides potential new avenues for attackers. When MCP first launched, it mandated session identifiers in URLs, a big security no-no. MCP originally also lacked message signing or verification mechanisms, which allows for message tampering.

Many of these vulnerabilities have been patched in subsequent updates, but there are others that are harder to eliminate. Plus, the fact that so many people [simply use MCP servers they find online](https://www.computerworld.com/article/4007724/openais-mcp-move-tempts-it-to-trust-genai-more-than-it-should.html) increases the risk of servers that are [misconfigured](https://www.darkreading.com/cloud-security/hundreds-mcp-servers-ai-models-abuse-rce) or plain [malicious](https://www.cyberark.com/resources/threat-research-blog/poison-everywhere-no-output-from-your-mcp-server-is-safe) going into production. For more on this topic, read “[MCP is fueling agentic AI—and introducing new security risks](https://www.csoonline.com/article/4015222/mcp-uses-and-risks.html)” at CSO Online.

#### MCP at a glance: 5 things you need to know <a href="#mcp-at-a-glance-5-things-you-need-to-know" id="mcp-at-a-glance-5-things-you-need-to-know"></a>

The Model Context Protocol (MCP) is changing how enterprise systems interact with AI. For IT leaders, knowing its implications is crucial for developing an efficient and secure AI strategy.

1. **Enables AI agency and automation**: MCP provides standardized infrastructure that allows AI models (like LLMs) to connect and interact with existing applications, databases, and services — that is, it’s foundational for building agentic AI that can autonomously perform complex tasks.
2. **Simplifies integration, reduces technical debt**: Prior to MCP, connecting AI to diverse enterprise systems meant custom, often-brittle integrations. MCP offers a plug-and-play framework designed to significantly reduce the development effort and technical debt associated with integrating AI. A single MCP server can expose a service to multiple AI clients.
3. **MCP is critical for AI context and accuracy**: LLMs need up-to-date, relevant context to avoid hallucinations and provide accurate, actionable responses. MCP servers bridge this gap by providing AI with real-time access to your proprietary, internal data and tools.
4. **Introduces new security considerations**: MCP also presents a new attack surface. IT leaders must be acutely aware of risks such as supply chain vulnerabilities, credential exposure and privilege abuse, and prompt/tool injection.
5. **Shifts enterprise AI architecture**: MCP is pushing enterprises toward AI-native architecture patterns. Rather than isolated AI experiments, MCP enables a unified, governed layer where AI can dynamically discover and interact with capabilities across your IT environment.

### What’s ahead for MCP? <a href="#whats-ahead-for-mcp" id="whats-ahead-for-mcp"></a>

Nevertheless, MCP’s utility and ease of use is overriding security concerns and ensuring that the technology will be around for some time to come. What’s on the horizon for this protocol? IBM’s Derks says that enterprises are starting to build tools and come up with strategies to manage proliferating MCP servers that AI agents will be making use of.

“One interesting pattern is the composition and orchestration of MCP servers,” Derks says. “What we see a lot is, instead of people connecting multiple MCP servers to a single client, they orchestrate multiple MCP servers into a single server and then connect that server to a client. If you think about clients on the left and servers on the right, there are some patterns emerging on the right to reduce the number of MCP servers needed, so as to reduce clutter—because if you provide 100 tools to a model, it’s going to get confused.

“That’s what I see a lot of the enterprise interest moving towards,” Derks adds. “How do you orchestrate and arrange all these servers, rather than how do you build the best possible client.” MCP is still in its early days, but expect enterprises to adapt quickly to keep up.

