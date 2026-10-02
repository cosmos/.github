# Cosmos Security Vulnerability Disclosure and Bug Bounty Policy

## Introduction

Cosmos Labs is committed to maintaining the security of the Cosmos Stack
and supporting responsible vulnerability disclosure. We operate a bug
bounty program to incentivize security researchers to identify and
report security issues.

This document defines the process for reporting vulnerabilities,
describes the bug bounty program, and outlines Cosmos Labs’ approach to
patching and public disclosure.

Security patches are made privately at every severity and published on a
monthly cycle. See [Security Patch Process](#security-patch-process).

------------------------------------------------------------------------

## Reporting a Vulnerability

**Private Disclosure Required**

Security vulnerabilities affecting the Cosmos ecosystem—including the
Cosmos SDK, CometBFT, IBC, and other core components—must be reported
privately through the channels listed below.

- **Preferred:** Submit reports through the
  [Cosmos Immunefi Bug Bounty Program](https://immunefi.com/bug-bounty/cosmos/information/).

> Reports submitted via email are *not eligible* for bounty rewards.
> Only reports submitted through the Bug Bounty qualify for bounties.

Public disclosure of vulnerabilities (including GitHub issues, blog
posts, or social media) is prohibited until Cosmos Labs has remediated
the issue and explicitly authorized disclosure.
Disclosure timelines may be coordinated with the reporter.

Submission of a report constitutes agreement to participate in
**coordinated vulnerability disclosure**, allowing time for development,
testing, and deployment of a fix prior to public release of details.

------------------------------------------------------------------------

## Bug Bounty Program Overview

Cosmos Labs operates a bug bounty program through **Immunefi**.
Eligible reports are rewarded based on severity, impact, and quality.

**In Scope:** Core Cosmos Stack components, including the Cosmos SDK,
CometBFT, IBC, Cosmos EVM, and other critical infrastructure components.

The authoritative scope definition, severity classifications, and
reward ranges are maintained on the [Cosmos Immunefi program
page](https://immunefi.com/bug-bounty/cosmos/scope/#top).

The program is governed by **Safe Harbor** provisions for good-faith research.
The Immunefi page defines the applicable **Coordinated Vulnerability Disclosure
Policy** and **Safe Harbor terms**.

> In the event of conflict, the Immunefi policy supersedes all other
> documentation.

------------------------------------------------------------------------

## Vulnerability Severity Levels

Reported vulnerabilities are assigned a severity classification that
determines handling priority and reward. It does not determine how or when a
fix is published: every severity follows the same patch process below.

The definitions that define severity classification and the reward ranges that
follow are maintained on our Immunefi page and not duplicated here. See the
[Cosmos Immunefi program
page](https://immunefi.com/bug-bounty/cosmos/scope/#top) Impacts in Scope
section for further details, as well as our [classification
framework](https://github.com/cosmos/security/blob/main/resources/CLASSIFICATION_MATRIX.md) for details on our
classification methodology.

------------------------------------------------------------------------

## Security Patch Process

All security patches are made privately, at every severity. There is no separate
public path for lower severity issues, and no severity-based decision about
where a fix lands.

The process does not depend on how we learned of the vulnerability. A report
through the bug bounty, a finding from an internal audit, and a direct
disclosure all follow the same path.

### Monthly private repositories

Cosmos Labs maintains a set of private repositories each month, one for each
repository in bug bounty scope. That includes the Cosmos SDK, CometBFT, IBC-Go,
Cosmos EVM and the CosmWasm repositories, and is not limited to them. They
are named for the repository and the month, for example
`cosmos-sdk-priv-july-2026`.

Access is granted to chain development teams and to major exchange partners.
Both must have passed KYC and run the repository in production. Each month's
repositories are created fresh and collaborators are reinvited, which keeps the
access list current. They are rebased periodically against `main` and the
maintained release branches, so they stay current with their public
counterparts.

### Patch detail

Patches merged into a private repository are not obfuscated. A patch PR carries
the description of the vulnerability, its root cause, severity, potential
impact, and any tests that reproduce it.

Downstream teams therefore hold the same information we do, and decide for
themselves how their chain responds. Cosmos Labs does not make that call on
their behalf.

### Private distribution

A team with access may use the patch however it needs to in order to build,
vendoring included. The restriction is on passing the source on: until the patch
is public, do not publish it in a public repository, do not commit it to one
inside a vendored dependency tree, and do not make it available to validators or
node operators.

A team that passes patch source to anyone outside the teams that already have
access, before the patch is public, loses access to the private repositories,
including the invitations to any future ones. This is a serious violation of
secure disclosure and puts all other chains at risk.

That is not a penalty under
[Consequences of Improper Disclosure](#consequences-of-improper-disclosure).
Access to the private repositories is granted on the condition that the source
stays private, so breaking that condition ends the grant. Those consequences may
apply as well, and we may take further action where the disclosure put chains at
risk.

Distributing a built binary that contains the patch is allowed, through whatever
mechanism the chain already uses, including GitHub release artifacts, provided no
patch source is included.

After each PR merges into a maintained release line, we tag a `-hotfix` version
on that line, so teams that want to remediate ahead of the public release can
build against the fix.

Where a fix spans repositories that depend on each other, downstream teams point
at the private repositories with `replace` directives in `go.mod`. Move those
back to the public tags at the monthly release. The private repository comes down
two weeks after that, and references to it stop resolving.

### Notification

Critical patches are announced by email to the security mailing list for the
affected repository. Patches at all other severities are not announced
individually. Teams track the `-hotfix` tags on the private repository and use
the patch details to determine whether to adopt a fix ahead of the public
release.

### Code freeze and public release

The private repositories are frozen for the last week of each month. During the
freeze no further patches are merged into that month's repositories. They go
into the next month's set instead, which is created seven days before the month
begins so that it is ready to receive them.

At the start of the following month, every patch in the frozen repositories is
merged into the corresponding public repository, new patch releases are tagged,
and a GHSA is published for each vulnerability with full details. That
publication is the disclosure date.

The frozen repositories stay up for two weeks after that release so teams can
move their builds across, and are deleted at the end of that window. Everything
in them is public by then, so the window holds nothing back. Access to the next
month's repositories is granted by fresh invitation on its own schedule, so the
retention does not extend anyone's access.

As an example, a vulnerability reported on August 12 and patched on August 15
reaches the private repositories the same week. Those repositories freeze on
August 25, their patches go public on September 1, and the repositories come
down on September 15.

### Critical vulnerabilities

Criticals follow the same process, with two additions. We notify affected chains
through the security mailing list for that repository that the month's private
repository contains a critical patch. And a critical is not merged into a
month's repository within three days of its freeze; one that would be goes into
the next month's repository instead, which pushes its public release out by a
month. Together with the week-long freeze, that leaves at least ten days between
a critical landing in a private repository and its public release.

### Active exploitation

A vulnerability that is being actively exploited, or where we confirm attacker
awareness ahead of the scheduled release, leaves the monthly cycle and is
handled immediately and separately as an incident. That means emergency
mitigations, private fix distribution, or a coordinated upgrade, ahead of any
public disclosure. This applies regardless of the original severity
classification.

------------------------------------------------------------------------

## Advisories

A GitHub Security Advisory is published for every patched vulnerability at the
monthly public release, containing:

- Vulnerability description
- Affected versions
- Severity classification
- Remediation guidance
- Reporter attribution (unless anonymity is requested)

All advisories remain publicly available.

------------------------------------------------------------------------

Cosmos Labs acknowledges and appreciates the contributions of security
researchers, auditors, and white-hat hackers who strengthen the Cosmos
ecosystem.

------------------------------------------------------------------------

## Consequences of Improper Disclosure

Publishing vulnerability details before their disclosure date puts users at
risk whatever the intent. A fix reaches downstream chains privately before it is
public, so early disclosure exposes chains that have not yet upgraded.

We give the reporter a disclosure date. That date is the one that counts, and
where we have not given one, it is the monthly public release that carries the
fix. Disclosure we have authorized is never a violation, whenever it happens.

The consequences below apply to anyone who publishes early, whether or not they
submitted through the bug bounty program.

Reporters who submit through Immunefi are also bound by the Immunefi publication
policy, which governs the program and supersedes this section where the two
conflict.

**1. Warning.** A first instance of publishing ahead of the disclosure date
results in a written warning from the security response team, recorded for the
purpose of judging later instances.

**2. Suspension.** A repeat instance removes the reporter from any private
notification or pre-disclosure list for 3 months. We continue to accept and act
on their vulnerability reports during that period, but those reports are not
eligible for a bounty.

**3. Permanent block.** A pattern of improper disclosure, or publishing with
intent to cause harm, results in a permanent block from the Cosmos organization
and from any pre-disclosure list, and no further reports from them are eligible
for a bounty. We still receive security reports from them, because closing that
channel would put users at risk, but they take no further part in the project.

At every level, a bounty already earned on the report that prompted the action
is still paid. What changes is eligibility going forward.

Severity determines the level rather than the number of prior instances, so a
sufficiently serious first instance may result in a suspension or a permanent
block without prior steps.

The security response team decides these actions. To appeal one, write to
[conduct@cosmos.network](mailto:conduct@cosmos.network), which is independent of
the team that imposed it.

------------------------------------------------------------------------

## Repository Scope

Supported components are the ones listed in the current
[release family](https://docs.cosmos.network/sdk/latest/release-family).
Repositories outside that list, including archived repositories, are not in
scope.
