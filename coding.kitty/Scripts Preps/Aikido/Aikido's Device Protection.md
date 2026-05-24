## Malicious Software, explained by cats.

### Refinement Prompt
> Good playtoy (fake mouse) vs bad playtoy (real mouse that steals food). Device Protection checks every toy before it gets through the door.
> we also detected a big attack recently - https://www.aikido.dev/blog/mini-shai-hulud-is-back-tanstack-compromised
> Mini Shai-Hulud is back. Like I said before, we were yet to see the full scale of the attack.
> The npm campaign we covered in April, when it targeted SAP packages, has now turned into a much larger compromise. Our Malware Team detected 373 malicious package-version entries across 169 npm package names.
> The basic goal is still the same: steal credentials from developer machines and CI/CD runners, then use those credentials to reach more packages.
> What changed is the scale and the release path. This wave does not just look like someone manually publishing bad versions. The malware is built to run inside build systems, steal npm and GitHub access, and abuse trusted publishing paths to push new compromised packages.
> If you read our earlier post, Mini Shai-Hulud Targets SAP npm Packages With a Bun-Based Secret Stealer, this is the follow-up: the same idea, but with a much bigger blast radius.
> What happened
> TanStack is still one of the most visible clusters, but it is no longer the whole story. The affected set now includes packages across @squawk, @tanstack, @uipath, @tallyui, @beproduct, @mistralai, @draftlab, @draftauth, @taskflow-corp, @tolka, and several unscoped packages.
> The largest clusters in this campaign are:
> @squawk: 87 package-version entries
> @tanstack: 83 package-version entries
> @uipath: 66 package-version entries
> unscoped packages: 39 package-version entries
> @tallyui: 30 package-version entries
> @beproduct: 18 package-version entries
> This list is still moving. You can see the full list of affected packages at the end of the blog. The important part is not only the number of packages, but where they run. These packages are likely to be installed in local developer environments, CI jobs, release workflows, and internal build systems.
> More about Aikido's device protection:
> What is Device Protection
> Device Protection gives you visibility and control over the software packages installed on your team's devices. It covers browser extensions, code libraries, IDE plugins, and build dependencies, all in one place.
> Use-cases
> Stop malware before it gets installed: Aikido automatically identifies known malicious packages and blocks them.
> Get full visibility into your software supply chain: See every browser extension, code library, IDE plugin, and build dependency installed across your team's devices.
> Lock down specific ecosystems: Block all installs from sources like Chrome or NPM when your team doesn't need them.
> Require approval for new packages: Let team members request installs while keeping admins in control of what gets approved.
> Getting Started
> Ready to start securing your devices from Malware and Malicious packages? Follow the steps below to deploy and configure Aikido Device for your environment.
> Deploy the Agent across your environment
> Deploy Aikido Device across your organization using MDM or a manual installation. If you need different configurations for different teams or devices, you can create multiple groups.
> Configure ecosystems
> Configure your package protection settings, including supported ecosystems, allowlists, blocklists, minimum package age, and group-specific exceptions.
> Supported ecosystems
> Device Protection monitors packages from the following sources:
> NPM
> PyPI (pip, poetry, uv, ..)
> VS Code
> Open VSX (Cursor, Windsurf, Kiro, ..)
> Maven
> NuGet
> Chrome extensions
> Skills.sh
> Go
> Integrate Aikido's device protection seemlessly in the script with the story. Keep the script under 1100 characters and ensure that it doesn't sound like an AD.