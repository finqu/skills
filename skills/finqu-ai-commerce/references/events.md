# Events

Events start work that is already configured. They are not a separate agent.

| Part | Role |
|------|------|
| Trigger | A store activity or a cron schedule |
| Target | A Worker (with instructions) or a Playbook (steps already defined) |

Merchants configure Events in Work Hub. Examples you can set up (they are not enabled for you):

- A sale → a Worker that summarizes what needs attention
- A new customer → a Playbook for that store's onboarding steps
- A cron → the Analyst with a daily summary

Choose a Worker for one task. Choose a Playbook when the response is an ordered sequence.

Manual and bulk Playbook runs do not need an Event. Admin REST webhooks notify your own HTTP endpoint. They are not Events. UCP order notifications are not Events either.

## Full reference

- [Events](https://developers.finqu.com/ai/ai-commerce/events.md.txt)
- [Webhooks](https://developers.finqu.com/apis-and-tools/rest-api/webhooks.md.txt)
