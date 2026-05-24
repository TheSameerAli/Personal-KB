Supply Chain Attack


What is it?

- It’s when the attack is done on external dependencies and not your code directly. This lets an attack backdoor or hack your app without ever getting access to your app.
- These vulnerabilitires can come from external packages such as npm or NuGet packages


Types of supply chain attacks:

- **Typosquatting**
	- Where attackers upload malicious packages to registries (npm, PyPI, RubyGems, etc.) using names nearly identical to popular librariees. These packages can be vulnerable.
	- Aikido can catch these typos early on.
- **Dependency confusion**
	- if an internal package name accidentally matches a package on the public registry, an attacker can publish a public package with the same name and a higher version to “confuse” the resolver
	- Aikido’s dependency scanner cross-references your package manifest against both public and private sources
- **Hijacked Libraries and Protestware**
	- When an attacker gains control of a legitimate package (via phishing the maintainer, stealing credentials, or exploiting lax security), they can publish a trojanized update that consumers download thinking it’s a normal new version.
	- Another scenario is “protestware,” where an open source maintainer intentionally alters their library (e.g., to display political messages or worse, to sabotage systems in certain countries). In both cases, the package that you’ve trusted and used for years can suddenly become a weapon against you.
	- Aikido’s continuous dependency monitoring shines here. Aikido’s scanner doesn’t just check for known CVEs – it also looks out for suspicious package behavior and known malware signatures in dependencies
- **Exposed Secrets and Credentials in Code or CI**
	- In the context of supply chain security, leaked secrets (API keys, cloud credentials, signing keys, etc.) can be as dangerous as any malware. Why? Because if an attacker finds a valid AWS key or CI/CD token in your GitHub repo or build logs, they can directly use it to **infiltrate your systems or poison your pipeline**
	- Aikido’s secrets detection feature is designed to catch exposed credentials early. It scans your code, config files, and even CI pipeline definitions for patterns that match API keys, private keys, tokens, and more.
- **Insecure CI/CD Pipeline Configurations**
	- Aikido provides CI/CD security scanning that helps audit your pipeline configurations for best practices and potential misconfigurations
- **Poisoned Pipeline Dependencies**
	- 