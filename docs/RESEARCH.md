# 公开资料与技术选型

资料核验日期：2026-11-16。以下区分公开文档描述与本项目拟接入方式。设备端效果由原型实测补充，费用与可用区域在调用前核对服务文档。

## 方言的三个环节

| 环节 | 公开文档描述 | 拟验证事项 |
| --- | --- | --- |
| 语音转文字 | 阿里云 Qwen-Audio 3.1 系列列有吴语；Paraformer v2 系列列有上海话 | 老派沪语、轻声、数字、混说与电视背景声 |
| 意图与回复 | 通用对话模型可用于意图提取与回复生成 | 提醒时间的澄清、短句表达、方言措辞审校 |
| 文字转语音 | 阿里云 Qwen-TTS 列有上海话 Jada 音色；CosyVoice 3 列有 Shanghai | 发音、语速、自然度与温和气质 |

带口音的普通话、完整沪语和混合表达分别测试；音色复刻与方言能力分别选型。开源模型的预置音色和云端音色库分别核对。

## 候选组件

### 阿里云语音服务

识别候选：qwen-audio-3.1-asr-flash-message、paraformer-realtime-v2。用同一组实录样本比较关键意图、时间识别和延迟。

合成候选：qwen3-tts-flash-realtime 或 qwen3-tts-flash 搭配 Jada。官方将该音色描述为“风风火火的沪上阿姐”，先试听再判断与小满性格的匹配度。支持方言指令的其他声音作为后续比较项。

- [语音识别概述](https://help.aliyun.com/zh/model-studio/asr-model)
- [实时语音识别](https://help.aliyun.com/zh/model-studio/real-time-speech-recognition-user-guide)
- [Qwen-TTS 音色列表](https://help.aliyun.com/zh/model-studio/qwen-tts-voice-list)
- [实时语音合成](https://help.aliyun.com/zh/model-studio/realtime-tts-user-guide)
- [Qwen-Audio-TTS 音色列表](https://help.aliyun.com/zh/model-studio/qwen-audio-tts-voice-list)

### 开源项目

| 项目 | 公开能力与拟用途 | 接入条件 |
| --- | --- | --- |
| [Silero VAD](https://github.com/snakers4/silero-vad) | 语音活动检测，提供 ONNX 方式；用于开始/结束讲话的分段 | 在目标设备测停顿阈值与速度；回声消除另行处理 |
| [D-Robotics hobot_audio](https://github.com/D-Robotics/hobot_audio) | 本地唤醒、命令词、ASR 与声源定位消息 | 示例采用 RDK X3 与配套阵列，实际板卡、系统和音频硬件先匹配 |
| [sherpa-onnx](https://github.com/k2-fsa/sherpa-onnx) | 关键词检测、VAD、识别和合成；支持 ARM64/Linux | 评估本地唤醒和小模型速度，BPU 适配另行验证 |
| [Qwen3-ASR](https://github.com/QwenLM/Qwen3-ASR) | 0.6B/1.7B 识别模型，30 种语言、22 种中国方言，包含吴语 | 备用路线先在独立电脑或服务器评估 |
| [CosyVoice](https://github.com/QwenAudio/CosyVoice) | CosyVoice 3 列出 18+ 中国方言/口音，包含上海话；有服务化示例 | 比较所选模型的声音与部署资源 |
| [xiaozhi-esp32](https://github.com/78/xiaozhi-esp32) | 离线唤醒、流式语音和设备交互参考；支持 AEC 的硬件可做全双工 | 外围设备按需评估，参赛原型以 RDK 为核心 |

官方 hobot_audio 建议中文唤醒词 3—5 字，可试“小满小满”，并实测修改后的效果。猫爪按键作为清楚的说话入口。

代码与模型权重的许可分别核对，记录实际采用的版本和出处。首版优先云端语音与本地实体交互，后续再评估自主部署。

## 产品与表达参考

| 参考 | 官方介绍重点 | 对小满的设计启发 |
| --- | --- | --- |
| [Qoobo / Petit Qoobo](https://qoobo.info/index-en) | 抚摸后摆尾；Petit 还包含声音回应和心跳触感 | 少量动作与恰当时机也能形成亲近感 |
| [Joy for All](https://agelessinnovation.com/joy-for-all/) | 面向老年人的互动陪伴宠物，强调陪伴、舒适和趣味 | 简洁操作、照料便利、日常易接近 |
| [LOVOT](https://lovot.life/) | “愛されるために生まれてきた”，表达为被爱而生 | 使用者也有表达喜欢、主动亲近的空间 |

小满据此形成自己的主张：“有伴的日子，刚刚好。”以触摸、家常和生活小事承载相处，原型验证聚焦交互可用性与再次互动意愿。
