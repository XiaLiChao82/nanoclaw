# Intent: Add onAutoRegister callback implementation

Add `onAutoRegister` callback to the channel options that auto-registers
groups when the bot is mentioned. The group is registered with the trigger
pattern set to `@${ASSISTANT_NAME}` and `requiresTrigger: true`.
