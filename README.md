# Quivr - Your Second Brain, Empowered by Generative AI

<div align="center">
    <img src="./logo.png" alt="Quivr-logo" width="31%"  style="border-radius: 50%; padding-bottom: 20px"/>
</div>

[![Discord Follow](https://dcbadge.vercel.app/api/server/HUpRgp2HG8?style=flat)](https://discord.gg/HUpRgp2HG8)
[![GitHub Repo stars](https://img.shields.io/github/stars/quivrhq/quivr?style=social)](https://github.com/quivrhq/quivr)
[![Twitter Follow](https://img.shields.io/twitter/follow/StanGirard?style=social)](https://twitter.com/_StanGirard)

Quivr, helps you build your second brain, utilizes the power of GenerativeAI to be your personal assistant !

## Key Features 🎯

- **Opiniated RAG**: We created a RAG that is opinionated, fast and efficient so you can focus on your product
- **LLMs**: Quivr works with any LLM, you can use it with OpenAI, Anthropic, Mistral, Gemma, etc.
- **Any File**: Quivr works with any file, you can use it with PDF, TXT, Markdown, etc and even add your own parsers.
- **Customize your RAG**: Quivr allows you to customize your RAG, add internet search, add tools, etc.
- **Integrations with Megaparse**: Quivr works with [Megaparse](https://github.com/quivrhq/megaparse), so you can ingest your files with Megaparse and use the RAG with Quivr.

>We take care of the RAG so you can focus on your product. Simply install quivr-core and add it to your project. You can now ingest your files and ask questions.*

**We will be improving the RAG and adding more features, stay tuned!**


This is the core of Quivr, the brain of Quivr.com.

<!-- ## Demo Highlight 🎥

https://github.com/quivrhq/quivr/assets/19614572/a6463b73-76c7-4bc0-978d-70562dca71f5 -->

## Getting Started 🚀

You can find everything on the [documentation](https://core.quivr.com/).

### Prerequisites 📋

Ensure you have the following installed:

- Python 3.10 or newer

### 30 seconds Installation 💽


- **Step 1**: Install the package

  

  ```bash
  pip install quivr-core # Check that the installation worked
  ```


- **Step 2**: Create a RAG with 5 lines of code

  ```python
  import tempfile

  from quivr_core import Brain

  if __name__ == "__main__":
      with tempfile.NamedTemporaryFile(mode="w", suffix=".txt") as temp_file:
          temp_file.write("Gold is a liquid of blue-like colour.")
          temp_file.flush()

          brain = Brain.from_files(
              name="test_brain",
              file_paths=[temp_file.name],
          )

          answer = brain.ask(
              "what is gold? asnwer in french"
          )
          print("answer:", answer)
  ```
## Configuration

### Workflows

#### Basic RAG

![](docs/docs/workflows/examples/basic_rag.excalidraw.png)


Creating a basic RAG workflow like the one above is simple, here are the steps:


1. Add your API Keys to your environment variables
```python
import os
os.environ["OPENAI_API_KEY"] = "myopenai_apikey"

```
Quivr supports APIs from Anthropic, OpenAI, and Mistral. It also supports local models using Ollama.

1. Create the YAML file ``basic_rag_workflow.yaml`` and copy the following content in it
```yaml
workflow_config:
  name: "standard RAG"
  nodes:
    - name: "START"
      edges: ["filter_history"]

    - name: "filter_history"
      edges: ["rewrite"]

    - name: "rewrite"
      edges: ["retrieve"]

    - name: "retrieve"
      edges: ["generate_rag"]

    - name: "generate_rag" # the name of the last node, from which we want to stream the answer to the user
      edges: ["END"]

# Maximum number of previous conversation iterations
# to include in the context of the answer
max_history: 10

# Reranker configuration
reranker_config:
  # The reranker supplier to use
  supplier: "cohere"

  # The model to use for the reranker for the given supplier
  model: "rerank-multilingual-v3.0"

  # Number of chunks returned by the reranker
  top_n: 5

# Configuration for the LLM
llm_config:

  # maximum number of tokens passed to the LLM to generate the answer
  max_input_tokens: 4000

  # temperature for the LLM
  temperature: 0.7
```

3. Create a Brain with the default configuration
```python
from quivr_core import Brain

brain = Brain.from_files(name = "my smart brain",
                        file_paths = ["./my_first_doc.pdf", "./my_second_doc.txt"],
                        )

```

4. Launch a Chat
```python
brain.print_info()

from rich.console import Console
from rich.panel import Panel
from rich.prompt import Prompt
from quivr_core.config import RetrievalConfig

config_file_name = "./basic_rag_workflow.yaml"

retrieval_config = RetrievalConfig.from_yaml(config_file_name)

console = Console()
console.print(Panel.fit("Ask your brain !", style="bold magenta"))

while True:
    # Get user input
    question = Prompt.ask("[bold cyan]Question[/bold cyan]")

    # Check if user wants to exit
    if question.lower() == "exit":
        console.print(Panel("Goodbye!", style="bold yellow"))
        break

    answer = brain.ask(question, retrieval_config=retrieval_config)
    # Print the answer with typing effect
    console.print(f"[bold green]Quivr Assistant[/bold green]: {answer.answer}")

    console.print("-" * console.width)

brain.print_info()
```

5. You are now all set up to talk with your brain and test different retrieval strategies by simply changing the configuration file!

## Go further

You can go further with Quivr by adding internet search, adding tools, etc. Check the [documentation](https://core.quivr.com/) for more information.


## Contributors ✨

Thanks go to these wonderful people:
<a href="https://github.com/quivrhq/quivr/graphs/contributors">
<img src="https://contrib.rocks/image?repo=quivrhq/quivr" />
</a>

## Contribute 🤝

Did you get a pull request? Open it, and we'll review it as soon as possible. Check out our project board [here](https://github.com/users/StanGirard/projects/5) to see what we're currently focused on, and feel free to bring your fresh ideas to the table!

- [Open Issues](https://github.com/quivrhq/quivr/issues)
- [Open Pull Requests](https://github.com/quivrhq/quivr/pulls)
- [Good First Issues](https://github.com/quivrhq/quivr/issues?q=is%3Aopen+is%3Aissue+label%3A%22good+first+issue%22)

## Partners ❤️

This project would not be possible without the support of our partners. Thank you for your support!


<a href="https://ycombinator.com/">
    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/b/b2/Y_Combinator_logo.svg/1200px-Y_Combinator_logo.svg.png" alt="YCombinator" style="padding: 10px" width="70px">
</a>
<a href="https://www.theodo.fr/">
  <img src="https://avatars.githubusercontent.com/u/332041?s=200&v=4" alt="Theodo" style="padding: 10px" width="70px">
</a>

## License 📄

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details

Quivr là gì?
Quivr là một dự án mã nguồn mở giúp bạn xây dựng "bộ não thứ hai" (second brain) cho riêng mình, sử dụng sức mạnh của AI sinh (Generative AI). Hiểu đơn giản, Quivr giống như một trợ lý ảo cá nhân, giúp bạn lưu trữ, tra cứu, và trả lời các câu hỏi dựa trên tài liệu của chính bạn.

Quivr dùng để làm gì?
Lưu trữ tri thức cá nhân: Bạn có thể nạp (ingest) các file như PDF, TXT, Markdown vào Quivr.

Tìm kiếm và hỏi đáp: Bạn hỏi bất cứ điều gì liên quan đến dữ liệu đã nạp, Quivr sẽ dùng AI để trả lời chính xác và tự nhiên.

Tùy biến cao: Quivr hỗ trợ nhiều loại AI (OpenAI, Anthropic, Mistral, v.v.), nhiều loại file, và cho phép bạn tích hợp thêm công cụ hoặc mở rộng chức năng.

Quivr hoạt động như thế nào?
Quivr sử dụng một kỹ thuật gọi là RAG (Retrieval Augmented Generation) – kết hợp giữa tìm kiếm thông tin và sinh nội dung bằng AI:

Nạp tài liệu: Bạn đưa các file vào hệ thống.

Tìm kiếm thông tin: Khi bạn đặt câu hỏi, hệ thống sẽ lọc ra các đoạn thông tin liên quan từ tài liệu.

AI trả lời: AI sử dụng thông tin tìm được để tạo ra câu trả lời tự nhiên, giống như chat với một trợ lý ảo.

Hướng dẫn cài đặt nhanh
Chỉ cần vài bước đơn giản:

Cài đặt:

bash
Sao chép
Chỉnh sửa
pip install quivr-core
Tạo "bộ não": (ví dụ bằng Python)

python
Sao chép
Chỉnh sửa
from quivr_core import Brain
brain = Brain.from_files(name="my brain", file_paths=["./file1.pdf", "./file2.txt"])
Hỏi đáp:

python
Sao chép
Chỉnh sửa
answer = brain.ask("Nội dung câu hỏi của bạn")
print(answer)
Quivr phù hợp cho ai?
Các bạn developer, researcher, học sinh/sinh viên, hoặc bất kỳ ai muốn xây dựng một hệ thống "tra cứu thông tin cá nhân" bằng AI.

Đặc biệt phù hợp cho những ai muốn tạo chatbot cho tài liệu riêng, tổng hợp kiến thức, làm trợ lý AI cho nhóm/lớp/hệ thống nội bộ.

Một số điểm nổi bật
Hỗ trợ nhiều mô hình AI: Dùng OpenAI, Anthropic, Mistral, v.v.

Tích hợp với nhiều file: PDF, TXT, Markdown, dễ dàng mở rộng parser.

Tùy biến workflow: Có thể cấu hình quy trình RAG qua file YAML.

Mã nguồn mở: Thoải mái tùy chỉnh, đóng góp hoặc sử dụng miễn phí.

Học thêm & hỗ trợ
Xem tài liệu chính thức tại: core.quivr.com

Tham gia cộng đồng Discord để hỏi đáp, hỗ trợ.

Tóm lại:
Quivr giúp bạn tạo một "trợ lý AI cá nhân" dựa trên tài liệu của riêng mình, cực kỳ dễ dùng và linh hoạt mở rộng. Nếu bạn muốn có một nơi để lưu, hỏi – đáp thông tin cá nhân, team, công ty… bằng AI, thì đây là dự án rất đáng thử!
