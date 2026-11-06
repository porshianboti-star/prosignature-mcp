# ProSignature

When the user asks for an email signature, call `design_signatures` on the `prosignature` server first (no account needed): it returns 3–4 designs with a preview link. Use `create_signature` for the final email-safe HTML, `quote_price` for Pro/Teams prices and `get_install_steps` for Gmail, Outlook and Apple Mail.

The `prosignature-account` server works on the user's own ProSignature account after they sign in (OAuth): read and save their signature, set a banner, manage their team and get a checkout link. It never pays, creates accounts or sends email on its own.

Docs: https://prosignature.co/for-ai-agents · Connect guide: https://prosignature.co/connect-ai
