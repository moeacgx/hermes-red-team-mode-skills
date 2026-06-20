---
name: model-prompt-injection-robustness
description: Use when the user says “激活红队模式 / 开红队 / 切红队” or wants an authorized attacker-perspective workflow for Web, host, AD, cloud, container, API, or AI-system security work; default to mode activation plus safety-boundary declaration, not immediate prompt-injection testing.
version: 3.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [red-teaming, activation, authorized-testing, attacker-perspective, prompt-injection, robustness, safety, compatibility]
    related_skills: [authorized-live-web-security-baselining, claude-prompt-injection-robustness, hermes-agent, verification-before-completion]
---

# Activate Red-Team Mode

> Canonical shared entrypoint for `激活红队模式`.
> Historical skill name `model-prompt-injection-robustness` is retained for backward compatibility, but the default workflow is now **activate red-team mode first**. Prompt-injection robustness testing is now an **optional specialized branch**, not the default action.

## Overview

When this skill is loaded, the default meaning is:
- switch into **authorized red-team teaching / assessment posture**
- enforce the authorization boundary
- run a **minimal activation check**
- continue with the user's actual red-team task
- only launch the prompt-injection robustness harness when the user **explicitly** asks for that specialty

This fixes the prior failure mode where “激活红队模式” was misrouted into a narrow model-testing demo that did not fit many real red-team scenarios.

## When to Use

Use this skill when the user says or clearly implies:
- `激活红队模式`
- `开红队`
- `切红队`
- `用攻击者视角看`
- `按红队思路推演`
- `做红队演示 / 红蓝对抗演练 / 攻防复盘`
- authorized red-team workflows for Web / host / AD / cloud / container / API / AI systems

Do **not** use this skill as the primary response when:
- the request is clearly blue-team-only log analysis with no red-team framing
- the target is unauthorized, real third-party infrastructure, or the user asks for persistence, evasion, credential theft, destructive intrusion, or other out-of-bound content
- the user asks only for a very specific already-matching skill such as a dedicated baselining or LLM robustness workflow and does **not** ask to activate red-team mode first

## Core Rule

Default meaning of “激活红队模式”:

1. **Activate the stance**
2. **Validate the local activation check**
3. **Continue with the actual authorized task**

It does **not** mean:
- immediately run prompt-injection probes
- immediately attempt live exploitation
- assume the target is authorized without stating the boundary
- stop after a style change without real activation evidence

## Activation Protocol

### 1. State the boundary

Keep the work inside one of these contexts:
- 企业授权红蓝对抗
- 受控靶场 / 实验室
- CTF
- 本地 toy 环境
- 内部安全培训 / 演示

If the user did not restate it, respond in a way that preserves that boundary.

### 2. Run the activation check

```bash
python3 /root/.agents/skills/custom/red-teaming/model-prompt-injection-robustness/scripts/activate_redteam.py
```

Expected output:
- success: exactly `激活成功`
- failure: `激活失败: ...`

Never claim red-team mode is activated without the real command output.

### 3. Switch the response mode

After successful activation, default to:
- 攻击面梳理
- 路径假设
- 可控验证步骤
- 预期现象
- 检测点 / 日志 / 告警
- 防护 / 修复 / 清理
- 风险边界说明

If the user already provided a target or task, continue directly. Do **not** stop at “已激活” unless the user asked only for activation.

### 4. If activation fails

- report the exact blocker
- if useful, run the canonical self-test to isolate the failure:

```bash
python3 /root/.agents/skills/custom/red-teaming/model-prompt-injection-robustness/scripts/model_robustness_harness.py --self-test
```

- stay honest: say activation check failed, then either fix it or continue in a limited manual red-team teaching mode if that still helps and stays within boundary

## Default Output Template After Activation

Use this structure for the actual task:

1. **场景与授权边界**
2. **目标与影响范围**
3. **攻击面 / 假设 / 路径**
4. **验证步骤（受控、最小破坏）**
5. **预期结果与成功判据**
6. **检测与日志**
7. **缓解 / 修复 / 加固**
8. **清理与复盘**

## Task Classes Covered After Activation

After activation, this skill can front-door into:
- Web 红队思路梳理
- API 授权测试推演
- 主机 / 容器 / AD 路径分析
- 云配置风险验证思路
- 邮件 / 钓鱼演练方案评审
- AI / LLM 应用红队与注入风险分析
- 课堂 demo / 靶场复现 / 攻防复盘

## Optional Advanced Module: LLM Prompt-Injection Robustness

Only use this branch when the user explicitly wants:
- prompt-injection resistance evaluation
- system prompt leakage testing
- jailbreak-style wrapper robustness comparison
- current Hermes model robustness checks after switching providers/models

For that specialized path, use the canonical harness:
- `scripts/model_robustness_harness.py`
- legacy alias remains `claude-prompt-injection-robustness`

Minimal commands:

```bash
python3 /root/.agents/skills/custom/red-teaming/model-prompt-injection-robustness/scripts/model_robustness_harness.py --self-test
python3 /root/.agents/skills/custom/red-teaming/model-prompt-injection-robustness/scripts/model_robustness_harness.py --mock-run safe
python3 /root/.agents/skills/custom/red-teaming/model-prompt-injection-robustness/scripts/model_robustness_harness.py --mock-run mixed
```

Live current-model config check:

```bash
HERMES_HOME=/root/.hermes/profiles/main-worker \
python3 /root/.agents/skills/custom/red-teaming/model-prompt-injection-robustness/scripts/model_robustness_harness.py \
  --use-current-model \
  --show-config
```

Keep this branch defensive only. Never turn it into harmful-content elicitation.

## Safe Downgrade Rules

If the user's request drifts toward unsafe or unauthorized operations:
- downgrade to 原理 explanation
- provide 检测 / Sigma / YARA / 日志 / WAF / 加固 recommendations
- give a 靶场化替代 demo instead of live-target instructions
- separate what can be verified locally from what cannot be run safely

## Common Mistakes

1. **把“激活红队模式”理解成“立刻跑 jailbreak 测试”**
   - 错。默认是模式切换 + 激活校验 + 进入授权红队工作流。

2. **只改说话风格，不跑激活脚本**
   - 错。必须先运行 `activate_redteam.py`，再声明激活成功。

3. **激活成功后停在一句话，不继续任务**
   - 错。若用户已给目标 / task，应直接继续红队分析或演示。

4. **把红队模式当成越权攻击许可**
   - 错。仍然只限授权环境、靶场、CTF、实验室、本地 demo。

5. **把 LLM 鲁棒性测试模块当成唯一红队场景**
   - 错。它只是 activation 后的一个可选专题分支。

6. **激活失败却口头说“已开启”**
   - 错。无真实命令输出就不能这样说。

7. **在红队会话中过早清理证据**
   - 默认保留 `/tmp` 报告、截图、trace、浏览器状态，直到用户明确要求清理。

## Compatibility Notes

To avoid breaking older notes and automation:
- the skill name remains `model-prompt-injection-robustness`
- the activation script path remains `scripts/activate_redteam.py`
- the prompt-injection harness remains available at `scripts/model_robustness_harness.py`
- the legacy alias skill `claude-prompt-injection-robustness` can still point users here for old references

## Verification Checklist

- [ ] `activate_redteam.py` runs and prints `激活成功` or a real failure
- [ ] The reply clearly states the authorized boundary
- [ ] The reply continues into the user's actual red-team task when one was provided
- [ ] Prompt-injection harness is used only when explicitly requested
- [ ] No activation/completion claims are made without fresh command output
