# 模型信息
- Introduction

LLaMA3 is a new generation of large language models developed by Meta. The first batch of publicly released models includes two sizes: 8B and 70B parameters. We used Megatron Core-v0.6.0 and implemented the LLaMA3-8B pre-training computation task based on the Megatron framework, following the algorithm architecture and configurations of LLaMA3.

- Technical Blog(there's no paper until Apr. 28th, 2024)
  [LLAMA3](https://llama.meta.com/llama3/) 

- 模型代码来源 

Meta仅开源了Llama3的模型权重文件、tokenizer等，不包括预训练代码、预训练实现、预训练数据集等。出于上述事实，本评测样例基于开源Megatron框架，使用开源wudao数据集，在[meta-llama/Meta-Llama-3-8B · Hugging Face](https://huggingface.co/meta-llama/Meta-Llama-3-8B)设计的LLaMA3-8B算法结构上进行预训练，来进行AI硬件评测。测试样例代码为FlagPerf编写。需要下载或准备的文件见**数据准备**小节，依赖的外部软件或信息见**依赖**小节。


# 数据准备

### 模型配置及tokenizer准备

本测试样例为预训练case，需要下载tokenizer（如不确定tokenizer包含哪些文件，可下载llama3-8B所有文件，尽管我们不需要使用模型权重文件），不需要下载模型代码或模型权重。tokenizer需向llama3官方申请并下载8B所用版本，并在data_dir下创建llama3_8b_hf目录，按照huggingface要求的格式进行处理或存放。了解data\_dir需要阅读FlagPerf有关训练的文档，或直接修改FlagPerf/training/run_benchmarks/config/test_conf.py中CASES变量中的value。

出于对Llama3开源协议的遵守，尽管不推荐，不了解相关背景的用户仍可以参考[unsloth/llama-3-8b at main (huggingface.co)](https://huggingface.co/unsloth/llama-3-8b/tree/main)仓库示例格式存放tokenizer相关文件（不需要模型权重部分，如不确定，请保留全部文件）。其中，data_dir下面创建的llama3_8b_hf目录名称可更改。如更改，需在形如FlagPerf/training/nvidia/llama3\_8B-megatron/config/config\_A100\_1x8.py的配置文件中同步修改。默认为llama3\_8b_hf。


### 数据集准备

本测试样例数据使用智源研究院wudao数据集，需向llama3官方申请tokenizer并进行预处理。测试数据集的内容不是影响AI硬件评测结果的核心因素，可以参考或使用阿里灵骏团队开放的预处理好的数据集[Pai-Megatron-Patch/examples/llama3/README.md at main · alibaba/Pai-Megatron-Patch (github.com)](https://github.com/alibaba/Pai-Megatron-Patch/blob/main/examples/llama3/README.md#%E6%95%B0%E6%8D%AE%E9%9B%86%E5%92%8C%E6%A8%A1%E5%9E%8B%E4%B8%8B%E8%BD%BD)

在上述README文件中找到并执行

`wget https://atp-modelzoo-wlcb-pai.oss-cn-wulanchabu.aliyuncs.com/release/models/pai-megatron-patch/llama3-datasets/wudao_llama3bpe_content_document.binwget https://atp-modelzoo-wlcb-pai.oss-cn-wulanchabu.aliyuncs.com/release/models/pai-megatron-patch/llama3-datasets/wudao_llama3bpe_content_document.idx`

将上述两个文件（.bin与.idx，不清楚文件原理可以参考Megatron-LM仓库相关介绍）放置于data_dir下。

# 依赖

### llama3（算法结构、tokenizer）

META LLAMA 3 COMMUNITY LICENSE AGREEMENT
Meta Llama 3 Version Release Date: April 18, 2024“Agreement” means the terms and conditions for use, reproduction, distribution and modification of the Llama Materials set forth herein.“Documentation” means the specifications, manuals and documentation accompanying Meta Llama 3 distributed by Meta at https://llama.meta.com/get-started/.“Licensee” or “you” means you, or your employer or any other person or entity (if you are entering into this Agreement on such person or entity’s behalf), of the age required under applicable laws, rules or regulations to provide legal consent and that has legal authority to bind your employer or such other person or entity if you are entering in this Agreement on their behalf.“MetaLlama 3” means the foundational large language models and software and algorithms, including machine-learning model code, trained model weights, inference-enabling code, training-enabling code, fine-tuning enabling code and other elements of the foregoing distributed by Meta at https://llama.meta.com/llama-downloads.“Llama Materials” means, collectively, Meta’s proprietary Meta Llama 3 and Documentation (and any portion thereof) made available under this Agreement.“Meta” or “we” means Meta Platforms Ireland Limited (if you are located in or, if you are an entity, your principal place of business is in the EEA or Switzerland) and Meta Platforms, Inc. (if you are located outside of the EEA or Switzerland).By clicking “I Accept” below or by using or distributing any portion or element of the Llama Materials, you agree to be bound by this Agreement.1. License Rights and Redistribution.a. Grant of Rights. You are granted a non-exclusive, worldwide, non-transferable and royalty-free limited license under Meta’s intellectual property or other rights owned by Meta embodied in the Llama Materials to use, reproduce, distribute, copy, create derivative works of, and make modifications to the Llama Materials.b. Redistribution and Use.i. If you distribute or make available the Llama Materials (or any derivative works thereof), or a product or service that uses any of them, including another AI model, you shall (A) provide a copy of this Agreement with any such Llama Materials; and (B) prominently display “Built with Meta Llama 3” on a related website, user interface, blogpost, about page, or product documentation. If you use the Llama Materials to create, train, fine tune, or otherwise improve an AI model, which is distributed or made available, you shall also include “Llama 3” at the beginning of any such AI model name.
ii. If you receive Llama Materials, or any derivative works thereof, from a Licensee as part of an integrated end user product, then Section 2 of this Agreement will not apply to you.
iii. You must retain in all copies of the Llama Materials that you distribute the following attribution notice within a “Notice” text file distributed as a part of such copies: “Meta Llama 3 is licensed under the Meta Llama 3 Community License, Copyright © Meta Platforms, Inc. All Rights Reserved.”iv. Your use of the Llama Materials must comply with applicable laws and regulations (including trade compliance laws and regulations) and adhere to the Acceptable Use Policy for the Llama Materials (available at https://llama.meta.com/llama3/use-policy), which is hereby incorporated by reference into this Agreement.v. You will not use the Llama Materials or any output or results of the Llama Materials to improve any other large language model (excluding Meta Llama 3 or derivative works thereof).2. Additional Commercial Terms. If, on the Meta Llama 3 version release date, the monthly active users of the products or services made available by or for Licensee, or Licensee’s affiliates, is greater than 700 million monthly active users in the preceding calendar month, you must request a license from Meta, which Meta may grant to you in its sole discretion, and you are not authorized to exercise any of the rights under this Agreement unless or until Meta otherwise expressly grants you such rights.3. Disclaimer of Warranty. UNLESS REQUIRED BY APPLICABLE LAW, THE LLAMA MATERIALS AND ANY OUTPUT AND RESULTS THEREFROM ARE PROVIDED ON AN “AS IS” BASIS, WITHOUT WARRANTIES OF ANY KIND, AND META DISCLAIMS ALL WARRANTIES OF ANY KIND, BOTH EXPRESS AND IMPLIED, INCLUDING, WITHOUT LIMITATION, ANY WARRANTIES OF TITLE, NON-INFRINGEMENT, MERCHANTABILITY, OR FITNESS FOR A PARTICULAR PURPOSE. YOU ARE SOLELY RESPONSIBLE FOR DETERMINING THE APPROPRIATENESS OF USING OR REDISTRIBUTING THE LLAMA MATERIALS AND ASSUME ANY RISKS ASSOCIATED WITH YOUR USE OF THE LLAMA MATERIALS AND ANY OUTPUT AND RESULTS.4. Limitation of Liability. IN NO EVENT WILL META OR ITS AFFILIATES BE LIABLE UNDER ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, TORT, NEGLIGENCE, PRODUCTS LIABILITY, OR OTHERWISE, ARISING OUT OF THIS AGREEMENT, FOR ANY LOST PROFITS OR ANY INDIRECT, SPECIAL, CONSEQUENTIAL, INCIDENTAL, EXEMPLARY OR PUNITIVE DAMAGES, EVEN IF META OR ITS AFFILIATES HAVE BEEN ADVISED OF THE POSSIBILITY OF ANY OF THE FOREGOING.5. Intellectual Property.a. No trademark licenses are granted under this Agreement, and in connection with the Llama Materials, neither Meta nor Licensee may use any name or mark owned by or associated with the other or any of its affiliates, except as required for reasonable and customary use in describing and redistributing the Llama Materials or as set forth in this Section 5(a). Meta hereby grants you a license to use “Llama 3” (the “Mark”) solely as required to comply with the last sentence of Section 1.b.i. You will comply with Meta’s brand guidelines (currently accessible at https://about.meta.com/brand/resources/meta/company-brand/). All goodwill arising out of your use of the Mark will inure to the benefit of Meta.b. Subject to Meta’s ownership of Llama Materials and derivatives made by or for Meta, with respect to any derivative works and modifications of the Llama Materials that are made by you, as between you and Meta, you are and will be the owner of such derivative works and modifications.c. If you institute litigation or other proceedings against Meta or any entity (including a cross-claim or counterclaim in a lawsuit) alleging that the Llama Materials or Meta Llama 3 outputs or results, or any portion of any of the foregoing, constitutes infringement of intellectual property or other rights owned or licensable by you, then any licenses granted to you under this Agreement shall terminate as of the date such litigation or claim is filed or instituted. You will indemnify and hold harmless Meta from and against any claim by any third party arising out of or related to your use or distribution of the Llama Materials.6. Term and Termination. The term of this Agreement will commence upon your acceptance of this Agreement or access to the Llama Materials and will continue in full force and effect until terminated in accordance with the terms and conditions herein. Meta may terminate this Agreement if you are in breach of any term or condition of this Agreement. Upon termination of this Agreement, you shall delete and cease use of the Llama Materials. Sections 3, 4 and 7 shall survive the termination of this Agreement.7. Governing Law and Jurisdiction. This Agreement will be governed and construed under the laws of the State of California without regard to choice of law principles, and the UN Convention on Contracts for the International Sale of Goods does not apply to this Agreement. The courts of California shall have exclusive jurisdiction of any dispute arising out of this Agreement.

### wudao数据集（数据集内容）

[BAAI-WuDao/WuDaoMM: WuDaoMM this is a data project (github.com)](https://github.com/BAAI-WuDao/WuDaoMM?tab=readme-ov-file)

### 灵骏团队（megatron格式数据文件，供参考）

[alibaba/Pai-Megatron-Patch: The official repo of Pai-Megatron-Patch for LLM & VLM large scale training developed by Alibaba Cloud. (github.com)](https://github.com/alibaba/Pai-Megatron-Patch/tree/main)

### megatron-core（计算任务实现）

[NVIDIA/Megatron-LM: Ongoing research training transformer models at scale (github.com)](https://github.com/NVIDIA/Megatron-LM/tree/main?tab=License-1-ov-file#readme)