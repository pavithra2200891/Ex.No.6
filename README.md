# Ex.No.6 Development of Python Code Compatible with Multiple AI Tools

# Date: 13.09.2025
# Register no. 212222050043
# Aim: Write and implement Python code that integrates with multiple AI tools to automate the task of interacting with APIs, comparing outputs, and generating actionable insights with Multiple AI Tools

#AI Tools Required:

# Explanation:
Experiment the persona pattern as a programmer for any specific applications related with your interesting area. 
Generate the outoput using more than one AI tool and based on the code generation analyse and discussing that. 

# Conclusion:

Integrating multiple AI tools for a task requires a programmatic approach that can manage API interactions, handle different data formats, and process outputs for a unified analysis. Python is an excellent choice for this due to its extensive ecosystem of libraries for both AI/ML and API communication.

# Python Code Implementation

The following is a conceptual Python code structure for a programmer persona focused on a specific application, such as generating and analyzing content. This example will use a hypothetical scenario of comparing two different large language models (LLMs) to generate content based on a user prompt, and then use a third AI-powered tool (like a sentiment analysis API) to analyze the generated outputs.


# Code:

import os
import requests
import json
from abc import ABC, abstractmethod

# Abstract Base Class for AI Tool
class AITool(ABC):
    def __init__(self, api_key, api_url):
        self.api_key = api_key
        self.api_url = api_url

    @abstractmethod
    def interact(self, prompt):
        pass

# Concrete class for LLM A (e.g., GPT-4)
class LLM_A(AITool):
    def interact(self, prompt):
        headers = {
            "Authorization": f"Bearer {self.api_key}",
            "Content-Type": "application/json"
        }
        data = {
            "model": "gpt-4-turbo",
            "messages": [{"role": "user", "content": prompt}]
        }
        try:
            response = requests.post(self.api_url, headers=headers, json=data)
            response.raise_for_status()
            return response.json()['choices'][0]['message']['content']
        except requests.exceptions.RequestException as e:
            print(f"Error interacting with LLM A: {e}")
            return None

# Concrete class for LLM B (e.g., Claude 3)
class LLM_B(AITool):
    def interact(self, prompt):
        headers = {
            "x-api-key": self.api_key,
            "Content-Type": "application/json"
        }
        data = {
            "model": "claude-3-opus-20240229",
            "messages": [{"role": "user", "content": prompt}]
        }
        try:
            response = requests.post(self.api_url, headers=headers, json=data)
            response.raise_for_status()
            return response.json()['content'][0]['text']
        except requests.exceptions.RequestException as e:
            print(f"Error interacting with LLM B: {e}")
            return None

# Concrete class for Sentiment Analysis Tool
class SentimentAnalyzer(AITool):
    def interact(self, text):
        headers = {
            "Authorization": f"Bearer {self.api_key}",
            "Content-Type": "application/json"
        }
        data = {
            "text": text
        }
        try:
            response = requests.post(self.api_url, headers=headers, json=data)
            response.raise_for_status()
            return response.json()['sentiment']
        except requests.exceptions.RequestException as e:
            print(f"Error interacting with Sentiment Analyzer: {e}")
            return None

# Main function to run the process
def main():
    # --- Persona: Content Strategist for a Tech Blog ---
    # Aim: Compare content from different AI models to find the most engaging and positive tone.
    # We will use two LLMs to generate blog post intros and a sentiment analyzer to score them.

    # 1. Initialize AI tools with API keys and URLs
    llm_a = LLM_A(api_key=os.getenv("LLM_A_API_KEY"), api_url="https://api.openai.com/v1/chat/completions")
    llm_b = LLM_B(api_key=os.getenv("LLM_B_API_KEY"), api_url="https://api.anthropic.com/v1/messages")
    sentiment_tool = SentimentAnalyzer(api_key=os.getenv("SENTIMENT_API_KEY"), api_url="https://api.sentiment-analyzer.com/v1/analyze")

    # 2. Define the user prompt
    user_prompt = "Write a compelling and positive blog post introduction about the future of renewable energy."

    # 3. Get outputs from multiple AI tools
    print("Generating content with LLM A...")
    output_a = llm_a.interact(user_prompt)
    
    print("\nGenerating content with LLM B...")
    output_b = llm_b.interact(user_prompt)

    # 4. Compare outputs and generate insights
    if output_a and output_b:
        print("\n--- Comparing Outputs ---")
        print("LLM A's Output:\n", output_a)
        print("\nLLM B's Output:\n", output_b)

        print("\n--- Generating Actionable Insights ---")
        
        # Analyze sentiment of each output
        print("Analyzing sentiment of LLM A's output...")
        sentiment_a = sentiment_tool.interact(output_a)
        
        print("Analyzing sentiment of LLM B's output...")
        sentiment_b = sentiment_tool.interact(output_b)

        print("\nSentiment Analysis Results:")
        print(f"LLM A Sentiment: {sentiment_a}")
        print(f"LLM B Sentiment: {sentiment_b}")
        
        # Actionable Insight
        if sentiment_a == 'positive' and sentiment_b == 'positive':
            if sentiment_a > sentiment_b: # Assuming a numerical score can be returned by the tool
                print("\nActionable Insight: LLM A produced a slightly more positive output. We should use its content or fine-tune LLM B to match its tone.")
            elif sentiment_b > sentiment_a:
                print("\nActionable Insight: LLM B's content is more positive. We should prioritize it for our blog post and a/b test it against LLM A's output.")
            else:
                print("\nActionable Insight: Both models are performing well. We can A/B test their content to see which one performs better with our audience.")
        else:
            print("\nActionable Insight: One or both models failed to produce a positive output. We need to refine our prompt engineering to achieve the desired tone.")

if __name__ == "__main__":
    main()

# Analysis and Discussion
The code demonstrates a practical application of prompt engineering and multi-AI tool integration.

# Code Generation Analysis
Modularity and Abstraction: The use of an AITool abstract base class and concrete classes (LLM_A, LLM_B, SentimentAnalyzer) promotes code reusability and maintainability. This design pattern, often called the Strategy pattern, allows us to easily swap out or add new AI models without changing the core logic of the main function. For a prompt engineering course, this highlights the importance of creating a scalable and organized codebase.

API Interaction: The requests library is a standard and robust way to interact with web APIs in Python. The code correctly handles headers and JSON payloads, which are common for RESTful API communication. It also includes basic error handling with try...except blocks and response.raise_for_status(), which is crucial for building reliable applications.

Persona-Driven Design: The "Content Strategist" persona gives a specific, real-world context to the generic task of comparing AI outputs. This makes the experiment more tangible and goal-oriented. The goal of finding a "compelling and positive tone" directly informs the selection of a sentiment analysis tool for comparison.

Discussion for a Prompt Engineering Course
This experiment illustrates several key concepts for prompt engineering:

Prompting is more than just text: The user_prompt is the core input, and its wording—"compelling and positive"—is a specific instruction that the AI models are expected to follow. The effectiveness of this prompt can be judged by the output of the sentiment analyzer.

The need for a "golden standard": The sentiment analysis tool acts as a form of automated evaluation or a "golden standard." Manually reading and comparing the outputs is subjective. An automated tool provides an objective metric (in this case, "positive," "negative," or "neutral") that can be used to compare models efficiently and at scale.

Actionable Insights: Simply getting two different outputs isn't enough. The final part of the code, which uses if/elif statements, translates the raw data (the sentiment scores) into actionable business insights. It tells the user not just what happened, but what to do next. This is the ultimate goal of AI integration: to drive decisions and actions.

The power of a pipeline: The entire script forms an automated pipeline:

# Input: User prompt.

Processing (Parallel): Generate content from multiple LLMs.

Processing (Sequential): Analyze each output with a separate tool.

# Output: 
Comparative analysis and an actionable recommendation.
This demonstrates how different AI tools can be chained together to perform a complex task that a single tool could not accomplish alone.

Building AI agents in Pure Python, without complex frameworks, is an excellent way to understand the core principles of AI integration.





# Result: The corresponding Prompt is executed successfully.
