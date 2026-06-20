# Hermes Red-Team Mode Skills

Shared Hermes skills for activating an authorized red-team workflow.

## Included skills

- `red-teaming/model-prompt-injection-robustness`
  - canonical entrypoint for `激活红队模式 / 开红队 / 切红队`
  - now defaults to **red-team mode activation + authorization boundary + task continuation**
  - keeps the prompt-injection robustness harness as an optional specialized branch
- `red-teaming/claude-prompt-injection-robustness`
  - backward-compatible alias for older notes and automation

## Key behavior

When the canonical skill is loaded for `激活红队模式`, the intended flow is:

1. state the authorized boundary
2. run the activation check
3. verify the real output
4. continue into the user's actual red-team task

Activation command:

```bash
python3 red-teaming/model-prompt-injection-robustness/scripts/activate_redteam.py
```

Specialized prompt-injection robustness self-test:

```bash
python3 red-teaming/model-prompt-injection-robustness/scripts/model_robustness_harness.py --self-test
```

## Notes

- This repo packages the current skill artifacts as a standalone GitHub repository.
- It is separate from the local `/root/.agents/skills/custom` directory, which is not itself a git repository on this machine.
