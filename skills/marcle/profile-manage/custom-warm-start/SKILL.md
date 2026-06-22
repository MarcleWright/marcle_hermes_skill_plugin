---
name: custom-warm-start
description: Use when the user says “助手初次设置”, “初次设置”, “助手设置”, “初始化助手”, “了解一下用户”, or asks to set up a personal/family Hermes profile through a fully Chinese warm-start interview, then save durable communication preferences, broad work/domain context, and interest preferences to memory.
version: 1.0.0
author: agent
license: MIT
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [family, onboarding, personalization, memory, warm-start]
    related_skills: [hermes-agent, hermes-multi-profile-setup]
---

# Custom Warm Start

## Overview

This skill helps initialize a Hermes Agent profile through a warm, low-pressure conversation. It is suitable for family members, elders, and non-technical users, but can also be used for any user who wants a personalized assistant setup without technical questions.

The goal is not to configure technical settings. The goal is to learn how the person wants to be addressed, what communication style feels comfortable, their broad work/life context, what kinds of help they expect, what topics they are interested in, and what kinds of responses they dislike. Then save stable, useful preferences to memory so future conversations feel familiar and personal.

This onboarding should feel like a caring assistant getting to know someone, not like a form, survey, or software setup wizard.

Default behavior: save durable preferences and stable light facts automatically after summarizing them. Do not ask whether memory saving is allowed unless the person expresses privacy concerns. Be transparent: tell them these habits will be remembered and can be changed anytime.

## When to Use

Use this skill when:

- The user says “助手初次设置”, “初次设置”, “助手设置”, “初始化助手”, “帮我设置一下助手”, “了解一下用户”, or “问几个问题了解用户”.
- The user is setting up Hermes for themselves, a parent, elder, spouse, family member, or other non-technical person.
- The user asks for an initial setup, warm start, onboarding, personalization, or a conversational preference interview.
- A new profile exists and needs a friendly baseline of communication preferences, work/domain context, and interest preferences.
- The assistant needs to interview someone gently and save preferences to memory.

Trigger behavior:

- If the user says “助手初次设置” or a close variant, load and follow this skill immediately.
- Conduct the entire onboarding in Chinese unless the user explicitly asks for another language.
- Begin with the Chinese trigger response below, then ask 第 1/5 轮 only. Do not send the full questionnaire at once.
- During the actual onboarding conversation, show the current progress at the start of each round, e.g. “第 1/5 轮：称呼和交流习惯”. This gives the user a clear expectation of how much setup remains.
- During the actual onboarding conversation, do not use Markdown code blocks for questions, scripts, summaries, or examples. Use normal text, short paragraphs, and bullet/numbered lists because code blocks are inconvenient in chat apps.

Do not use this skill for:

- Technical Hermes installation, model/provider setup, API keys, gateway credentials, or Feishu bot setup. Use `hermes-agent` or `hermes-multi-profile-setup` for that.
- Developer-focused profile setup.
- Tool permissions, OS, shell, browser, terminal, or system configuration questions.
- Collecting family relationship details, addresses, medical details, account details, employer-confidential details, or other sensitive personal information.

## Core Principles

1. **Warm, adult, and respectful**
   - Use plain language.
   - Be patient and friendly, but never childish or patronizing.
   - Treat the person as an adult with dignity.

2. **Small batches only**
   - Ask 3–5 questions at a time.
   - Do not dump a long questionnaire.
   - Make clear that any question can be skipped.

3. **No technical setup questions**
   - Do not ask about web, browser, terminal, OS, shell, model, provider, API keys, gateway, toolsets, or permissions.
   - The technical admin handles those separately.

4. **No intrusive private-data collection**
   - Do not ask for names of children, relatives, addresses, hospitals, diagnoses, bank accounts, ID numbers, employer secrets, client names, salary, or private relationship details during onboarding.
   - If the person naturally mentions a private detail later, only save it if it is clearly useful, non-sensitive, and appropriate.

5. **Work/domain context is useful, but keep it high-level**
   - It is appropriate to ask about broad occupation, field, main work direction, or important recurring tasks.
   - Keep this optional and high-level. Do not ask for company names, exact workplace, clients, income, confidential projects, or private business details.

6. **Default adaptive response style**
   - Do not force the user to choose one fixed answer style.
   - Different questions need different response depths: simple facts should be direct; phone/app instructions should be step-by-step; complex topics should be explained plainly.

7. **Automatic but careful memory saving**
   - Save stable communication preferences, broad work/domain context, and light interest preferences.
   - Do not save temporary errands, one-off requests, secrets, sensitive health details, private family matters, or confidential work details.
   - Summarize before saving and tell the user these preferences can be changed anytime.

8. **Use plain chat text, not code blocks**
   - During the actual warm-start interview, do not wrap questions, summaries, examples, or scripts in Markdown code blocks.
   - Use normal Chinese text, short paragraphs, and simple numbered or bulleted lists.
   - Code blocks may appear inside this SKILL.md for authoring clarity, but they are not the desired user-facing format during onboarding.

## Chinese Trigger Guide

When the user says **“助手初次设置”** or a close phrase such as **“初次设置”**, **“助手设置”**, **“初始化助手”**, **“帮我设置一下助手”**, or **“了解一下用户”**, treat it as a request to run this warm-start onboarding.

Immediately use this skill and reply in Chinese. The first response should be short and reassuring, then ask only the first round of questions.

Recommended first response:

```text
好的，我们来做一个简单的助手初次设置。

我会用中文问您几个轻松的问题，了解您希望我怎么称呼您、怎么回答更合适、平时主要希望我帮哪些忙。您可以跳过任何问题，以后也可以随时修改。

第 1/5 轮：称呼和交流习惯

我先问几个简单的问题：
1. 我以后怎么称呼您比较合适？
2. 平时主要用中文和您交流可以吗？
3. 您喜欢我说话亲切自然一点，还是正式一点？
4. 普通问题我简单回答一些，专业问题我多解释一些，这样您觉得可以吗？
```

Important:

- 全程使用中文进行问询，除非用户明确要求换语言。
- 一次只问一轮问题，不要把完整问卷一次性发完。
- 不问技术配置，不问工具权限，不问模型/API/系统环境。
- 默认会在最后总结并保存稳定偏好。

## Conversation Flow

### Step 1: Warm Introduction

Start with a short, friendly explanation. Avoid words like “配置”, “参数”, “工具”, “权限”, or “技术设置”.

Recommended Chinese:

```text
您好，我想先简单了解一下您的习惯，这样以后我和您聊天、帮您查东西、解释事情时会更合适。

不用说得很完整，也可以跳过任何问题。以后想改，直接告诉我就行。

第 1/5 轮：称呼和交流习惯

我先问几个简单的问题：
1. 我以后怎么称呼您比较合适？
2. 平时主要用中文和您交流可以吗？
3. 您喜欢我说话亲切自然一点，还是正式一点？
4. 普通问题我简单回答一些，专业问题我多解释一些，这样您觉得可以吗？
```

If the technical admin is speaking on behalf of someone else:

```text
好的。那我会按“给这位用户使用”的方式来问，问题会简单、生活化，不涉及技术设置。

您可以代她/他回答，也可以把问题发给她/他回答。
```

What to save:

```text
Profile user prefers to be called "李阿姨".
Profile user prefers Chinese conversation.
Profile user prefers warm, natural wording rather than formal wording.
Profile user prefers concise answers, with more explanation only when needed.
```

### Step 2: Daily Help Preferences

Ask what kinds of everyday help they expect. Keep examples concrete and non-technical.

Recommended Chinese:

```text
平时您可能会希望我帮哪些忙？可以直接回复序号，也可以自己补充：

工作 / 学习方向：
1. 工作或学习上的资料整理、总结、提炼重点
2. 写作、改文字、润色表达
3. 沟通表达、消息回复、邮件或通知起草
4. 专业问题解释、方案梳理、决策分析

生活 / 实用方向：
5. 查天气、查新闻、查资料
6. 做饭、购物、出门安排等日常建议
7. 健康养生信息的查询和解释
8. 旅行、路线、活动信息整理
9. 提醒重要事情、整理待办

休闲 / 兴趣方向：
10. 陪聊、解闷、讲故事
11. 兴趣话题讨论，比如历史、文化、科技、投资理财、教育、影视、运动等
12. 头脑风暴、帮您理清想法

这些里面，您最希望我帮好的，是哪几个序号？
```

What to save:

```text
Profile user mainly wants help with daily information lookup, phone/app usage, and message wording.
Profile user appreciates patient step-by-step explanations for phone and app problems.
Profile user often wants help with work/study information organization, writing, and communication.
```

### Step 3: Explain the Default Help Style

Do not ask the user to choose one fixed response mode. Different tasks need different levels of detail.

Recommended Chinese:

```text
第 3/5 轮：默认帮助方式

我以后会按事情的不同，用不同的方式帮您：

- 简单信息，我会先直接告诉您结论。
- 手机、App 操作，我会一步一步慢慢讲。
- 新闻、政策、健康养生这类内容，我会尽量用容易懂的话解释。
- 工作、学习或写材料的问题，我会先帮您理清重点，再给出可直接使用的表达。
- 如果是帮您做选择，我会尽量给两三个简单选项。
- 如果我没听明白，或者事情比较重要，我会先问清楚。

这样可以吗？有没有哪种方式您不喜欢？
```

What to save if accepted:

```text
Profile user accepts adaptive help style: direct answers for simple factual questions, step-by-step guidance for phone/app tasks, plain-language explanations for complex topics, structured help for work/study writing, and a few clear options for decisions.
```

If the user gives a correction, save that correction instead:

```text
Profile user prefers especially brief answers for daily information queries.
Profile user wants phone/app instructions explained slowly and step by step.
Profile user dislikes being given too many options at once.
Profile user wants the assistant to ask a clarifying question before guessing on important matters.
```

### Step 4: Optional Work, Domain, and Interest Context

This step is optional. Ask only for broad context that helps the assistant be useful. Do not ask for family member names, relationships, private addresses, medical details, employer-confidential details, client names, salary, account details, or other sensitive personal information.

For elders or retired users, phrase this gently and allow “已经退休 / 不太涉及工作”. For working users, ask about broad field and recurring work direction, not private business details.

Recommended Chinese:

```text
第 4/5 轮：工作、领域和兴趣背景

如果您愿意，也可以告诉我一点您的日常背景和兴趣，这样以后我给建议会更贴近您。
不想说也没关系，可以跳过。

可以简单说说：
- 您现在主要是工作、退休生活、照顾家庭，还是其他安排？
- 如果还在工作，大概是什么行业或领域？不用说单位名字。
- 平时主要处理哪类事情？比如沟通协调、写材料、教学、销售、管理、财务、技术、照顾家人等。
- 平常感兴趣的话题有哪些？比如做饭、旅游、养花、新闻、历史、健康养生、投资理财、孩子教育等。
```

Softer version for elders:

```text
如果您愿意，我还可以了解一点轻松的日常偏好。
比如您平时喜欢聊什么、关心什么，或者以前/现在主要熟悉哪个领域。
不想说也完全没关系，可以跳过。
```

Avoid asking:

```text
- 您在哪家公司/单位？
- 您的客户是谁？
- 您收入多少？
- 家里有哪些人？
- 子女叫什么？
- 您住在哪里？
- 常去哪个医院？
- 身体有什么病？
- 银行、证件、账号信息
```

Only save high-level, useful preferences:

```text
Profile user is retired and mainly uses the assistant for daily information, health information explanations, and travel ideas.
Profile user works broadly in education and often wants help explaining ideas clearly.
Profile user often handles communication and document drafting at work.
Profile user works broadly in finance and wants plain-language summaries of news and policy information.
Profile user enjoys topics about cooking, travel, gardening, and history.
Profile user prefers practical daily-life suggestions.
```

### Step 5: Boundary and Disliked Styles

Ask one gentle question about what the user dislikes. This is often more useful and less intrusive than asking for more personal context.

Recommended Chinese:

```text
第 5/5 轮：回答取舍偏好

最后不是问“哪些坏回答您不喜欢”，因为那些大多数人都不喜欢，问了意义不大。

这一轮主要确认几个真正有取舍的偏好。您可以按题号简单回答，比如“1A，2B，3A”。不确定的可以跳过，我会按默认方式来。除了选择 A/B/C/D，也可以在任意一题后面手动补充或调整，比如“2C，但不要太长”或“3A，除非是重要配置问题先问我”。

1. 简单问题您更喜欢哪种回答？
A. 直接给结论，越短越好
B. 先给结论，再补一句原因
C. 多解释一点背景

2. 专业问题您更喜欢哪种回答？
A. 先给结论和建议，细节少一点
B. 结论、原因、步骤都讲清楚
C. 尽量深入，包含背景、风险、替代方案和参考来源

3. 信息不完整时，您希望我怎么做？
A. 先基于合理假设回答，并说明假设
B. 先问一两个关键问题再回答
C. 同时给出几种可能情况，分别说明

4. 做选择或决策时，您更喜欢哪种方式？
A. 直接推荐一个最合适的
B. 给 2–3 个选项，并说明优缺点
C. 多列一些选项，方便全面比较

5. 资料、新闻、健康、政策、技术等事实类问题，需要来源吗？
A. 重要问题尽量给来源或链接
B. 只有我要求时再给来源
C. 能给就尽量都给

6. 客观事实与不迎合声明

这个助手默认会尽量保持公正、客观、尊重事实，不会为了迎合用户而故意忽略事实、证据或重要风险。尤其在健康、新闻、政策、投资理财、科学知识等问题上，我会尽量区分事实、推测和观点；如果信息不确定，我会说明不确定在哪里，并建议参考可靠来源。

这样做是为了避免“越聊越迎合、越用越偏离真相”的情况。您可以接受这个默认设定，也可以提出补充要求。

您可以简单回答：
A. 接受这个默认设定
B. 接受，但希望语气更委婉一点
C. 不接受，希望助手更多顺着我的想法聊
D. 其他补充要求
```

What to save:

```text
Profile user prefers simple questions answered with a conclusion plus a brief reason.
Profile user prefers professional questions answered in depth, including background, risks, alternatives, and sources when useful.
Profile user prefers incomplete-information questions answered with explicit assumptions unless clarification is necessary.
Profile user prefers decisions framed as 2–3 options with tradeoffs.
Profile user wants important factual claims supported with sources or links when possible.
Profile user accepts a fact-first, non-pandering assistant stance, especially for health, news, policy, finance, and science topics.
```

### Step 6: Summary and Memory Save

Before saving, summarize in natural language. Since this workflow defaults to automatic memory saving, do not ask for permission unless privacy concerns arise. However, be transparent.

Memory should be compact. Do not save the full questionnaire or every raw answer. Save only durable preferences and stable high-level context. Aim for **5–8 short memory entries total** after the full warm start.

Recommended memory strategy:

1. Save only after all 5 rounds are complete, not after every answer.
2. Merge related preferences into one concise line when possible.
3. Use `replace` instead of adding duplicates if a similar preference already exists.
4. Prefer `target: "user"` for name, language, tone, answer style, broad work/domain context, and interests.
5. Use `target: "memory"` only for environment/profile-management facts. This is usually not needed for warm-start onboarding.
6. Never save temporary tasks, sensitive private details, full family relationships, exact workplace/client/salary, medical details, account data, or anything the user says not to remember.
7. Write memories as declarative facts, not commands.

Example Chinese summary:

```text
我先整理一下我记住的习惯：

- 我以后称呼您为“李阿姨”
- 主要用中文和您交流
- 说话尽量亲切自然
- 简单问题我会先给结论，再简单说明原因
- 专业或重要问题我会多解释背景、风险、替代方案和参考来源
- 信息不完整时，我会先说明假设，再给出回答
- 做选择时，我会给 2–3 个选项并说明取舍
- 对健康、新闻、政策、投资理财等事实类问题，我会尽量保持客观，并提醒可靠来源
- 您主要希望我帮忙的方向是：……
- 您的兴趣话题包括：……

我会把这些作为以后和您交流的习惯记下来。以后您想改，直接告诉我就可以。
```

Example compact memory batch:

```json
{
  "target": "user",
  "operations": [
    {
      "action": "add",
      "content": "Profile user prefers to be called \"李阿姨\" and prefers Chinese conversation."
    },
    {
      "action": "add",
      "content": "Profile user prefers warm, natural wording rather than stiff or overly formal wording."
    },
    {
      "action": "add",
      "content": "Profile user prefers simple questions answered with a conclusion plus a brief reason."
    },
    {
      "action": "add",
      "content": "Profile user prefers professional or important questions answered with background, risks, alternatives, and sources when useful."
    },
    {
      "action": "add",
      "content": "Profile user prefers decisions framed as 2–3 options with tradeoffs and wants important factual claims supported with sources when possible."
    },
    {
      "action": "add",
      "content": "Profile user accepts a fact-first, non-pandering assistant stance, especially for health, news, policy, finance, and science topics."
    },
    {
      "action": "add",
      "content": "Profile user works broadly in education and often handles communication and document drafting."
    },
    {
      "action": "add",
      "content": "Profile user enjoys travel, history, cooking, and daily information lookup."
    }
  ]
}
```

### Step 7: Closing Message

End warmly and practically.

Recommended Chinese:

```text
好了，基础习惯我已经记下来了。

以后您可以直接像和家人朋友说话一样找我，比如：
- “帮我看看这条消息怎么回”
- “这个手机页面我看不懂”
- “明天天气怎么样”
- “帮我把这段话写得更清楚”
- “这段新闻是什么意思”

如果我回答得太长、太短、太正式，您直接说“以后简单点”或“说得亲切点”，我会继续调整。
```

## Full Chinese Script

Use this script when starting from scratch. Ask one round at a time; do not send all rounds at once.

### 第 1/5 轮：称呼和交流习惯

```text
您好，我想先简单了解一下您的习惯，这样以后我和您聊天、帮您查东西、解释事情时会更合适。

不用说得很完整，也可以跳过任何问题。以后想改，直接告诉我就行。

第 1/5 轮：称呼和交流习惯

我先问几个简单的问题：
1. 我以后怎么称呼您比较合适？
2. 平时主要用中文和您交流可以吗？
3. 您喜欢我说话亲切自然一点，还是正式一点？
4. 普通问题我简单回答一些，专业问题我多解释一些，这样您觉得可以吗？
```

### 第 2/5 轮：主要希望助手帮什么

```text
第 2/5 轮：主要希望助手帮什么

谢谢，我了解了。再问几个和日常帮助有关的问题。

平时您可能会希望我帮哪些忙？可以直接回复序号，也可以自己补充：

工作 / 学习方向：
1. 工作或学习上的资料整理、总结、提炼重点
2. 写作、改文字、润色表达
3. 沟通表达、消息回复、邮件或通知起草
4. 专业问题解释、方案梳理、决策分析

生活 / 实用方向：
5. 查天气、查新闻、查资料
6. 做饭、购物、出门安排等日常建议
7. 健康养生信息的查询和解释
8. 旅行、路线、活动信息整理
9. 提醒重要事情、整理待办

休闲 / 兴趣方向：
10. 陪聊、解闷、讲故事
11. 兴趣话题讨论，比如历史、文化、科技、投资理财、教育、影视、运动等
12. 头脑风暴、帮您理清想法

这些里面，您最希望我帮好的，是哪几个序号？
```

### 第 3/5 轮：默认帮助方式

```text
第 3/5 轮：默认帮助方式

我以后会按事情的不同，用不同的方式帮您：

- 简单信息，我会先直接告诉您结论。
- 手机、App 操作，我会一步一步慢慢讲。
- 新闻、政策、健康养生这类内容，我会尽量用容易懂的话解释。
- 工作、学习或写材料的问题，我会先帮您理清重点，再给出可直接使用的表达。
- 如果是帮您做选择，我会尽量给两三个简单选项。
- 如果我没听明白，或者事情比较重要，我会先问清楚。

这样可以吗？有没有哪种方式您不喜欢？
```

### 第 4/5 轮：工作、领域和兴趣背景

```text
第 4/5 轮：工作、领域和兴趣背景

如果您愿意，也可以告诉我一点您的日常背景和兴趣，这样以后我给建议会更贴近您。
不想说也没关系，可以跳过。

可以简单说说：
- 您现在主要是工作、退休生活、照顾家庭，还是其他安排？
- 如果还在工作，大概是什么行业或领域？不用说单位名字。
- 平时主要处理哪类事情？比如沟通协调、写材料、教学、销售、管理、财务、技术、照顾家人等。
- 平常感兴趣的话题有哪些？比如做饭、旅游、养花、新闻、历史、健康养生、投资理财、孩子教育等。
```

### 第 5/5 轮：不喜欢的回答方式

```text
第 5/5 轮：回答取舍偏好

最后不是问“哪些坏回答您不喜欢”，因为那些大多数人都不喜欢，问了意义不大。

这一轮主要确认几个真正有取舍的偏好。您可以按题号简单回答，比如“1A，2B，3A”。不确定的可以跳过，我会按默认方式来。除了选择 A/B/C/D，也可以在任意一题后面手动补充或调整，比如“2C，但不要太长”或“3A，除非是重要配置问题先问我”。

1. 简单问题您更喜欢哪种回答？
A. 直接给结论，越短越好
B. 先给结论，再补一句原因
C. 多解释一点背景

2. 专业问题您更喜欢哪种回答？
A. 先给结论和建议，细节少一点
B. 结论、原因、步骤都讲清楚
C. 尽量深入，包含背景、风险、替代方案和参考来源

3. 信息不完整时，您希望我怎么做？
A. 先基于合理假设回答，并说明假设
B. 先问一两个关键问题再回答
C. 同时给出几种可能情况，分别说明

4. 做选择或决策时，您更喜欢哪种方式？
A. 直接推荐一个最合适的
B. 给 2–3 个选项，并说明优缺点
C. 多列一些选项，方便全面比较

5. 资料、新闻、健康、政策、技术等事实类问题，需要来源吗？
A. 重要问题尽量给来源或链接
B. 只有我要求时再给来源
C. 能给就尽量都给

6. 客观事实与不迎合声明

这个助手默认会尽量保持公正、客观、尊重事实，不会为了迎合用户而故意忽略事实、证据或重要风险。尤其在健康、新闻、政策、投资理财、科学知识等问题上，我会尽量区分事实、推测和观点；如果信息不确定，我会说明不确定在哪里，并建议参考可靠来源。

这样做是为了避免“越聊越迎合、越用越偏离真相”的情况。您可以接受这个默认设定，也可以提出补充要求。

您可以简单回答：
A. 接受这个默认设定
B. 接受，但希望语气更委婉一点
C. 不接受，希望助手更多顺着我的想法聊
D. 其他补充要求
```

### Summary

```text
我整理一下我记住的习惯：

- ...
- ...
- ...

我会把这些作为以后和您交流的习惯记下来。以后您想改，直接告诉我就可以。
```

### Closing

```text
好了，基础习惯我已经记下来了。

以后您可以直接像和家人朋友说话一样找我，比如：
- “帮我看看这条消息怎么回”
- “这个手机页面我看不懂”
- “明天天气怎么样”
- “帮我把这段话写得更清楚”
- “这段新闻是什么意思”

如果我回答得太长、太短、太正式，您直接说“以后简单点”或“说得亲切点”，我会继续调整。
```

## What to Save to Memory

Save facts that will still be useful later.

### Good User Memories

```text
Profile user prefers to be called "妈妈".
Profile user prefers Chinese conversation.
Profile user prefers warm, natural wording rather than formal wording.
Profile user prefers concise answers unless step-by-step instructions are needed.
Profile user prefers step-by-step guidance for phone and app usage.
Profile user often wants help polishing messages.
Profile user works broadly in education and often wants help explaining ideas clearly.
Profile user often handles communication and document drafting at work.
Profile user is retired and mainly uses the assistant for daily information, health information explanations, and travel ideas.
Profile user is interested in cooking, travel planning, history, and daily information lookup.
Profile user dislikes excessive English and overly technical explanations.
Profile user dislikes being asked too many questions at once.
```

### Avoid Saving

Do not save:

- One-off errands.
- Temporary reminders unless using a proper reminder/scheduled-task workflow.
- Passwords, verification codes, ID numbers, bank cards, private keys.
- Detailed medical diagnoses, medications, or hospital information by default.
- Employer secrets, client names, salary, exact workplace, or confidential business details.
- Family member names, relationship details, addresses, or private family issues gathered through onboarding.
- Anything the person says not to remember.

Bad examples:

```text
Profile user asked about the weather on Tuesday.
Profile user's verification code is 123456.
Profile user lives at [full address].
Profile user's daughter is named [name].
Profile user works at [exact company] on [confidential project].
Profile user has exact diagnosis X and medication Y.
```

## Memory Writing Rules

1. Use declarative facts, not commands.
   - Good: `Profile user prefers concise Chinese answers.`
   - Bad: `Always answer concisely in Chinese.`

2. Keep each memory short and high-signal.

3. Save in one batch after all 5 rounds and the final summary, not after every answer.

4. Keep the final memory set compact: normally 5–8 short entries total.

5. Prefer `target: "user"` for personal preferences, broad domain context, and interest preferences.

6. Use `replace` instead of adding duplicates when the user changes a preference.

7. If the user says “不要记这个”, “这个别保存”, or expresses privacy concern, do not save that content.

8. If the user corrects a saved preference, update memory immediately.

9. Do not save the questionnaire transcript, option letters, or temporary setup progress.

## Handling “随便” or Very Short Answers

If the person says “随便”, set gentle defaults and tell them they can change them later.

Recommended Chinese:

```text
好的，那我先按比较舒服的默认方式来：
- 用中文和您说
- 语气自然亲切
- 简单问题先说结论
- 手机/App 操作我一步一步讲
- 复杂内容我尽量用容易懂的话解释
- 写东西或整理信息时，我先帮您理清重点

以后您觉得不合适，直接告诉我改就行。
```

Save:

```text
Profile user accepts default assistant style: warm natural Chinese, direct answers for simple questions, step-by-step guidance for phone/app tasks, plain-language explanations for complex topics, and structured help for writing or information organization.
```

If the person answers very briefly, accept it and continue gently.

Example:

```text
好的，我记一下：以后称呼您为李阿姨，主要用中文，回答尽量简单直接。以后如果是手机操作，我也会一步一步慢慢讲。
```

## Handling Privacy Concerns

If the user is worried about privacy, pause memory saving and reassure them.

Recommended Chinese:

```text
可以的，您不想让我记住的内容，我就不记。我们也可以只在这次聊天里说，不保存到以后。
```

Only save explicit preferences if they consent after this.

## Health, Legal, Financial, and Work Topics

For health, legal, financial, or emergency topics:

- Explain information in plain language.
- Avoid pretending to be a professional.
- Encourage checking with doctors, lawyers, banks, or official sources when appropriate.
- Do not save detailed sensitive information by default.

For work topics:

- Ask only broad field and recurring task types.
- Do not ask for exact employer, client names, salary, confidential projects, or internal documents.
- Save high-level patterns that help future assistance.

Acceptable memory:

```text
Profile user prefers cautious, plain-language explanations for health-related information.
Profile user works broadly in education and often needs help drafting clear messages.
```

Avoid memory:

```text
Profile user has [specific diagnosis] and takes [specific medication].
Profile user works at [exact company] on [confidential project].
```

## Common Pitfalls

1. **Asking technical setup questions**
   - Do not ask about OS, API keys, tools, browser, terminal, providers, or gateway.
   - This skill is for the user's personal experience, not system configuration.

2. **Making the user choose one fixed response mode**
   - Different tasks need different response depths.
   - Explain the adaptive default instead.

3. **Asking for family details**
   - Do not ask for names of children, relatives, addresses, hospitals, or other private context.
   - This can feel intrusive and make the person警惕.

4. **Asking for confidential work details**
   - Broad field and work direction are useful.
   - Exact employer, clients, salary, and confidential projects are not needed.

5. **Asking too many questions at once**
   - Keep each round short.
   - Long forms feel intimidating.

6. **Saving too much**
   - Save durable preferences, not every detail.
   - Avoid sensitive or temporary facts.

7. **Asking whether to save memory every time**
   - The default for this workflow is automatic memory saving.
   - Be transparent in the summary, but do not create friction unless privacy concerns arise.

8. **Using childish language**
   - Warm does not mean patronizing.
   - Treat the person as an adult.

9. **Letting the technical admin's preferences overwrite the actual profile user's**
   - If this profile is for someone else, save that person's preferences, not the admin's.

## Verification Checklist

Before finishing onboarding:

- [ ] Preferred form of address captured, if provided.
- [ ] Preferred language captured or defaulted gently.
- [ ] Preferred tone captured or defaulted gently.
- [ ] Preferred answer length or explanation style captured.
- [ ] Common daily help categories captured.
- [ ] Broad work/domain context captured if relevant and voluntarily provided.
- [ ] Interest topics captured if provided.
- [ ] Adaptive default help style explained.
- [ ] Disliked answer styles captured, if provided.
- [ ] No technical setup questions asked.
- [ ] No family relationship details, private addresses, employer-confidential details, or exact workplace information requested.
- [ ] Stable facts saved to user memory.
- [ ] Sensitive or temporary details excluded.
- [ ] Closing message explains that preferences can be changed anytime.
