# Abstract

This project introduces an AI-driven automation system designed to analyze, interpret, and
visualize software repositories using Large Language Models (LLMs) and Retrieval-Augmented
Generation (RAG) techniques. The system accepts a GitHub repository link as input and
automatically performs key operations, including repository cloning, file structure analysis,
semantic content extraction, and contextual summarization of source code.

At its core, the project integrates workflow automation through n8n, enabling modular and
rapid prototyping of each stage—from data retrieval and preprocessing to embedding
generation and AI-based interpretation. The use of RAG ensures that the LLM receives precise
and relevant code segments, allowing for accurate explanations, summaries, and project
insights without exceeding token limits.

In addition to textual analysis, the system also generates mind maps and visual representations
of repository structures, enhancing developer comprehension and code navigation. Once the
n8n-based prototype is validated, its exported workflow (in JSON format) can be
programmatically transformed into a Python-based, scalable pipeline, enabling deployment as
a standalone AI agent or API service.

By combining automation, intelligent retrieval, and generative reasoning, this project
demonstrates how AI can be applied to improve code understanding, documentation, and
onboarding efficiency for developers and organizations alike.