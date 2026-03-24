# Outcome-Based Security Assurance
*An exploration of deriving assurance from system behavior rather than control artifacts.*

Security outcomes are determined by how systems operate.

However, most security assurance today is derived from control frameworks and audit artifacts. These provide standardized, auditable signals, but often fail to reflect how systems actually function under real conditions.

This project explores a minimal model for deriving assurance directly from system behavior, using outcome-based evaluation and structured reasoning instead of control checklists.

It is not a replacement for existing frameworks, but an attempt to improve how security outcomes are evaluated in practice.

**Status: v0.1 (exploratory model)**

---

## 0. Summary
Security assurance today is largely derived from abstract controls and shallow evidence. This produces signals that are often weak or misleading with respect to how systems actually operate.

This project explores a minimal model for deriving assurance directly from system behavior, using outcome-based evaluation and structured reasoning instead of control checklists.

## 1. The Problem
Security outcomes are determined by how systems behave.

Human factors and organizational processes matter, but their impact is expressed through system design, configuration, and operation. This model focuses on the parts of security that are realized in system behavior.

* identity is defined and enforced in IAM systems
* access decisions are made in code and configuration
* data protection depends on how services are built and deployed
* detection depends on the signals that are collected and how they are interpreted

Assurance is produced somewhere else. It is defined, evaluated, and communicated through a separate set of control frameworks, processes, and incentives.

* control frameworks define generalized control requirements
* policies and procedures describe intended behavior
* audits evaluate evidence against those requirements

These control frameworks and audit processes are designed to be broadly applicable and consistently auditable across many organizations, not to reflect the specifics of any one system. 

Even where controls are tailored to a specific organization, the underlying model remains control-based and artifact-driven. Customization affects how controls are expressed, but does not change the underlying evaluation model or how assurance is derived.

As a result:

* control definitions are necessarily generic
* requirements are not tightly coupled to specific threats or architectures
* evaluations focus on whether controls can be demonstrated

The underlying incentives reinforce this:

* controls must be easy to describe
* evidence must be easy to collect and verify
* evaluations must be repeatable across many environments

Taken together, this produces processes that are effective at generating consistent audit artifacts, but poorly suited to guiding real security decisions or evaluating actual security outcomes.

Consequently, these control-based assurance frameworks are not used in practice to design systems or reason about system security. They provide standardized coverage for external assurance purposes, but not practical security direction or guidance. They answer “do you have something that resembles this control” rather than “does this system actually achieve intended security outcomes.” They should not be mistaken for a model for building or evaluating secure systems.

In practice, security work is structured differently:

* security teams build systems based on threats and constraints
* security programs are informed by frameworks such as NIST CSF to reason about structural coverage, and by standards such as OWASP ASVS to ensure those capabilities are realized in code and configuration
* more mature teams incorporate adversary-informed models such as MITRE ATT&CK to evaluate detection and response coverage against real attacker behavior
* compliance frameworks are applied after the fact to map those systems to generalized controls
* auditors evaluate artifacts derived from those controls

The mapping is lossy.

Security exists in specific systems with concrete identities, trust boundaries, data flows, and failure modes. Compliance frameworks translate that into generalized controls and evidence, reducing fidelity and changing what is being optimized.

First, it discards information:

* controls are not tied to specific threats or architectures
* important distinctions between implementations are flattened
* edge cases and failure modes are not represented

Second, it introduces new optimization criteria that are only weakly related to actual security:

* controls must be easy to describe in generalized terms
* evidence must be easy to collect and verify
* evaluations must be consistent and repeatable across organizations

These properties are necessary for auditability, but they do not correspond to how systems fail or how attackers operate.

As a result:

* it is possible to satisfy control requirements while leaving meaningful attack paths unaddressed
* it is possible for strong, well-designed systems to be poorly reflected in audit artifacts
* assurance artifacts do not reliably indicate how a system will behave under real conditions

This creates a structural gap.

Assurance is intended to represent security, but it is produced through a separate layer with different goals and constraints. In practice, these artifacts are often treated as indicators of a company’s security posture and used as proxies for security in vendor evaluation and procurement decisions.

Because they are not designed to reflect how systems actually behave under real conditions, this can drift into a form of security theater, where the artifact is sound but the underlying signal is weak. The measurements are precise and repeatable, but often orthogonal to actual security outcomes.

Assurance evaluates a distorted representation of security, not security itself.

## 2. Design Principles

These follow directly from the failure modes of control-based assurance.

* Assurance must be derived from system state
* Outcomes must map to real threats
* Evaluation must be structured, not reduced to checklists
* Evidence must be grounded in observable system behavior
* The model must be usable by engineers

## 3. The Model
This model is structured around three elements:

Outcome → Reasoning Dimensions → System Evidence

The goal is to determine whether a security outcome is actually achieved, based on direct reasoning about system behavior rather than inferred from the presence of controls or artifacts.

* Outcomes define what must be true for the system to be considered secure.
* Reasoning dimensions define the aspects of the system that must be understood and evaluated to determine whether the outcome holds.
* System evidence consists of the concrete configurations, behaviors, and signals that support that evaluation.

These elements are used together to evaluate security directly at the level where it is realized: in system behavior.

### 3.1 Outcomes

Outcomes define what must be true for a system to be considered secure.

Examples:

* Unauthorized access is prevented
* Sensitive data is protected in transit and at rest
* Malicious activity is detected in a timely manner

Outcomes describe the security property that must hold. They do not prescribe implementation or specific controls.

### 3.2 Reasoning Dimensions
Reasoning dimensions define what must be understood about a system to determine whether a given outcome actually holds.

For each outcome, a set of dimensions is used to structure evaluation. These dimensions ensure that the analysis covers the parts of the system where that outcome can succeed or fail.

Example:

**Outcome:**

Unauthorized access is prevented

**Reasoning Dimensions:**

* Identity model
* Authorization model
* Systems in scope
* Enforcement points
* Bypass paths
* Provisioning and deprovisioning
* Logging and detection coverage

These dimensions ensure that evaluation covers the parts of the system where the outcome can succeed or fail, defining the space of reasoning required to assess it.

This is a different model than control-based assurance:

* Controls attempt to specify what must exist.
* Reasoning dimensions specify what must be understood.

They provide structure without reducing evaluation to a checklist, and allow different systems to be evaluated on their actual behavior rather than their similarity to a predefined control.

To illustrate how a reasoning dimension is used in practice:

**Identity model**

This dimension captures how identities are defined, established, and trusted within the system.

Evaluation involves questions such as:

* What constitutes an identity (user, service, device)?
* How are identities created and verified?
* What are the sources of truth for identity?
* How are identities authenticated?
* What assumptions are made about identity validity?

The goal is not to check whether an “identity control” exists, but to understand how identity actually works in the system, where it can fail, and how those failures could lead to unauthorized access.

### 3.3 System Evidence
System evidence consists of the concrete configurations, behaviors, and signals used to support evaluation. 

Examples include:

* IAM configuration
* access logs
* authentication and authorization flows
* enforcement mechanisms
* audit trails

System evidence is drawn directly from the system and used to evaluate each reasoning dimension, rather than relying on separately maintained artifacts or documentation.

Claims about the system should be traceable to observable system state or behavior, grounding evaluation in what the system actually does rather than how it is described.

In practice, evaluation is typically based on walkthroughs of the system with the team, supported by direct observation of configurations, flows, and outputs. This does not require exhaustive artifact collection or continuous evidence capture.

Evaluators validate claims selectively where needed, through inspection, demonstration, or sampling. Rather than requiring uniform coverage across all systems, evaluation can be focused on areas of higher complexity, uncertainty, or systemic risk.

## 4. Evaluation Model
Evaluation proceeds outcome by outcome.

For each outcome, the evaluator works through the associated reasoning dimensions to understand how the system behaves and whether the outcome holds.

For each dimension, the evaluator:

* assesses whether the organization can demonstrate a coherent understanding of how the relevant aspect of the system works
* identifies and makes explicit gaps, assumptions, and unknowns in the understanding of the system, and where these limit confidence in the outcome
* evaluates, based on system evidence and the preceding reasoning, whether system behavior supports the outcome

This evaluation model is more in-depth than a control-based audit, but it is not intended to require exhaustive expertise in every system or configuration.

The evaluator’s role is not to verify specific settings or best practices across all technologies, but to assess whether the system’s security properties are well understood and supported by evidence. In practice, this is the assurance companion to a threat model: threat modeling defines how a system is intended to work and where it can fail, while this evaluation assesses whether those failure modes are actually addressed in the system as implemented.

This does not require deep expertise in every underlying technology. The evaluation can be performed by someone with a strong foundation in security and threat modeling, working from system descriptions together with direct observation of system state and behavior (such as configuration, execution paths, and system outputs), rather than detailed knowledge of specific tools or configurations.

The goal is to determine whether the system actually achieves the outcome, based on reasoning about system behavior rather than the presence of predefined controls.

This is not a pass/fail checklist. It is a structured assessment grounded in system behavior, where conclusions are supported by direct evidence and explicit reasoning.

### Use of Guidance 

This model does not rely on checklists or predefined control requirements as the basis for evaluation.

However, evaluators are not expected to start from scratch. In practice, reference guides can be used to structure thinking about common system patterns (for example, OAuth integrations, identity systems, or data pipelines).

These guides serve as prompts for reasoning, not as checklists to be completed. They help ensure that important dimensions are considered, but they do not define the outcome or replace analysis.

The evaluator is still responsible for understanding the specific system, identifying how it actually behaves, and determining where it may fail.

### Scope and Practical Application

This model does not require exhaustive analysis of all systems or continuous re-evaluation of unchanged components.

In practice, evaluation is scoped and iterative:

- Evaluations are typically performed on a subset of systems relevant to the outcome, not the entire environment  
- Systems with higher complexity, risk, or uncertainty receive more attention than well-understood or stable components  
- Previously evaluated systems do not need to be re-analyzed in full unless there are meaningful changes  
- Understanding can be accumulated over time, rather than rebuilt from scratch for each evaluation  

This allows evaluation effort to be concentrated where it is most valuable, rather than uniformly distributed.

The goal is not to produce a complete model of the entire system landscape on a fixed cadence, but to develop and maintain an accurate understanding of how critical systems behave with respect to key security outcomes.

Over time, this results in a progressively refined understanding of the system, rather than a series of disconnected, point-in-time assessments.

## 5. Example Evaluations
This model is best understood through a concrete example.

The following examples evaluate a specific security outcome using reasoning dimensions and system evidence.

Each example follows the same structure:

* Outcome
* Reasoning dimensions
* System walkthrough
* Observations
* Gaps

Not every reasoning dimension will produce explicit observations or gaps in every evaluation.

The purpose of these dimensions is to ensure comprehensive analysis, not to require uniform output. In practice, some dimensions may be well understood or low-risk and require little discussion, while others surface meaningful uncertainty or potential failure modes.

Completeness is determined by whether each dimension has been considered, not whether it results in a visible finding.

The goal is to show how evaluation works in practice, using a real or representative system rather than a hypothetical control.

This model does not attempt to reduce conclusions to simple pass/fail outcomes. In many cases, system behavior cannot be accurately represented as a binary result without discarding important context.

Instead, conclusions reflect how the system behaves, including where that behavior depends on assumptions, incomplete validation, or areas of uncertainty. This is intentional: preserving that context provides a more accurate representation of security than forcing a definitive judgment where one cannot be justified.

### Example: Internal Administrative Access

**Outcome:**

Unauthorized access is prevented.
The system should ensure that only appropriately authorized personnel can perform administrative actions, and that those actions cannot be used to access or modify resources beyond their intended scope.

**Reasoning Dimensions:**

* Identity model
* Authorization model
* Systems in scope
* Enforcement points
* Bypass paths
* Provisioning and deprovisioning
* Logging and detection coverage

**System Walkthrough:**

Consider a SaaS application with an internal administrative interface used by support and engineering teams.

Employees authenticate through the company’s identity provider (for example, Okta or Google Workspace) and access an internal admin panel.

The admin panel allows authorized users to:

* view and modify customer data
* reset credentials or authentication factors
* impersonate users for troubleshooting

Access to the admin panel is restricted based on role (for example, support, engineer, admin), and actions are logged.

In this system:

* Identity is established through the company’s identity provider
* Authorization is based on internal roles and permissions assigned to employees
* Administrative actions are performed through the admin panel and associated backend services

**Observations:**

* Administrative access is mediated through the internal admin panel and associated APIs
* Employees can perform actions on behalf of users, including impersonation and direct data access
* Authorization appears to be role-based, with different levels of access for support and engineering roles
* The admin panel and backend services act as the primary enforcement points for administrative actions
* Logging captures administrative actions performed through the interface, but attribution and completeness are not yet established

**Gaps:**

* It is unclear whether all administrative actions are consistently enforced through the same authorization layer, or whether some backend services can be accessed directly
* The scope and boundaries of roles (for example, support vs. engineer) are not fully defined, leaving uncertainty about whether access is appropriately limited
* Impersonation functionality may allow actions that bypass user-level authorization constraints, depending on how enforcement is implemented
* Provisioning and deprovisioning of administrative access were not examined, creating uncertainty about whether access is promptly removed when no longer needed
* Logging captures actions, but it is unclear whether logs are sufficient to attribute actions to individual users and detect misuse

**Notes:**

This evaluation does not depend on whether an “access control” or “least privilege” control exists.

It evaluates whether administrative access, as implemented, is actually constrained to intended use, and whether the system reliably prevents misuse or overreach in practice.

#### Sample Output (Internal Administrative Access)

**Outcome:**

Unauthorized access is prevented

**Conclusion:**

Administrative access appears to be mediated through the admin panel and role-based permissions, and actions are logged. This suggests that access is generally constrained to authorized personnel. However, correctness depends on consistent enforcement across all administrative paths and clear definition of role boundaries.

**Confidence:**

Moderate. The primary access paths and enforcement points are understood, but some aspects of authorization consistency and role definition were not fully verified.

**Key points:**

* Administrative actions are performed through a centralized interface and associated backend services
* Access is controlled through roles assigned to employees
* Impersonation and direct data access are supported and form part of normal workflows

**Limitations:**

* It is unclear whether all administrative actions are consistently enforced through the same authorization layer
* The scope and boundaries of roles are not fully defined or verified
* It is unclear whether impersonation bypasses user-level authorization constraints
* Provisioning and deprovisioning of administrative access were not examined

### Example: OAuth Integration

**Outcome:**

Unauthorized access is prevented.
The system should ensure that only intended users and services can obtain and use access tokens, and that those tokens cannot be used to access resources beyond their intended scope.

**Reasoning Dimensions:**

* Identity model
* Authorization model
* Systems in scope
* Enforcement points
* Bypass paths
* Provisioning and deprovisioning
* Logging and detection coverage

**System Walkthrough:**

Consider a SaaS application that allows users to connect a third-party service via OAuth (for example, integrating with Google Workspace, Slack, or GitHub).

While using the application, the user initiates a connection to an external service (for example, Google, Slack, or GitHub), authenticates with that service, and grants the application specific permissions (scopes).

The external service then provides the application with an access token (a credential that allows the application to act on behalf of the user) and, in some cases, a refresh token (which allows new access tokens to be obtained without further user interaction).

Tokens are stored and used by the application’s server-side components to access the external service’s API (for example, to read data or perform actions) on behalf of the user, within the granted permissions.

In this system:

* Identity is established through the external service the user authenticated with
* Authorization is based on granted permissions (scopes) and application-level access controls
* The application’s server-side components act on behalf of users using stored tokens

**Observations:**

* Identity is delegated to the external service, and tokens appear to be the primary mechanism used to represent that identity within the system
* Authorization is based on scopes (the permissions granted during connection), which are carried through to API access
* Tokens are stored and used by the application’s server-side components to act on behalf of users without additional interaction
* Token storage and handling appear to be the primary enforcement point for access, so incorrect validation or handling would directly impact authorization across the system

**Gaps:**

* It is unclear whether the application’s server-side components are subject to the same authorization constraints as user-initiated flows, creating a potential path for actions that bypass user-level checks
* Token validation behavior was not directly observed across all services, leaving uncertainty about whether identity and scope are consistently enforced at each access point
* The consistency of scope enforcement between services was not verified
* Token lifetime and revocation behavior were not fully examined
* Logging may not clearly attribute actions performed via tokens back to the originating user

**Notes:**

This evaluation does not depend on whether an “OAuth control” exists.

It evaluates whether the system, as implemented, actually prevents unauthorized access given how tokens are issued, validated, stored, and used.

The outcome depends on how these elements interact in practice, not on the presence of any individual control.

#### Sample Output (OAuth Integration)

**Outcome:**

Unauthorized access is prevented

**Conclusion:**

Based on the walkthrough, access appears to be restricted according to user-granted permissions, and tokens are used as the primary mechanism for enforcing authorization across the system. This suggests the intended authorization model is generally followed, but correctness depends on consistent token validation and enforcement across services.

**Confidence:**

Moderate. The overall flow and primary enforcement points are understood, but key aspects of token validation and cross-service enforcement were not directly observed.

**Key points:**

* Access is mediated through tokens issued by the external service and used by the application’s server-side components
* Permissions are determined at connection time and carried through to API access
* Token handling appears to be the primary enforcement mechanism for authorization across the system

**Limitations:**

* Token validation behavior was not directly observed across all services, leaving uncertainty about whether identity and scope are consistently enforced
* The consistency of scope enforcement between services was not verified
* It is unclear whether the application’s server-side components are subject to the same authorization constraints as user-initiated flows, creating a potential path for bypassing user-level checks
* Token lifetime and revocation behavior were not fully examined

## 6. What This Is Not
This model has a narrower scope than existing assurance frameworks.
It is not:

* a replacement for all compliance frameworks
* a complete control catalog
* a mechanical checklist
* a finished or standardized framework

It may use structured guides to support analysis, but these are prompts for reasoning, not requirements to be satisfied.

Its purpose is to provide a more direct way to evaluate whether security outcomes are actually achieved, based on how systems behave, rather than to define or validate a set of prescribed controls.

## 7. What This Could Become
This model is not a complete framework, but it suggests a direction.

It could serve as:

* a basis for more meaningful audits grounded in system behavior
* an input into continuous or periodic assurance models
* a bridge between security engineering and assurance

This model defines how outcomes are evaluated, not which outcomes must be included.

Any practical application would require an agreed scope: a set of outcomes that provides sufficient coverage of relevant security concerns. Existing frameworks such as NIST CSF and OWASP ASVS can help define that scope, while this model provides a way to evaluate whether those outcomes are actually achieved.

Over time, advances in tooling may make this model more practical to apply. For example, read-only AI systems with access to relevant system state could help assemble system walkthroughs, trace claims to evidence, identify inconsistencies or gaps, and maintain accumulated understanding across evaluations. This would not replace judgment, but could reduce the effort required to perform structured, system-based assurance.

The goal is not to replace existing structures, but to improve how assurance is derived and interpreted. Over time, approaches like this may inform how assurance models evolve, particularly in areas where current methods fail to reflect how systems actually behave.

Further evolution of this model would likely require additional structure around how conclusions are used in practice.

This could include:

* lightweight ways to aggregate or summarize conclusions across outcomes without reducing them to simple pass/fail results  
* guidance for how evaluators communicate confidence, uncertainty, and limitations to different audiences  
* approaches for incorporating this model into existing assurance processes without requiring full replacement  
* mechanisms for accumulating understanding over time and tracking meaningful changes in system behavior  

These are intentionally not defined here. The goal of this model is to establish a foundation for evaluating outcomes based on system behavior; how that evaluation is operationalized is an area for further exploration.

## 8. Limitations
This model has practical limitations.

* It requires evaluators with strong security fundamentals and the ability to reason about real systems, even without deep expertise in every underlying technology
* It is harder to standardize than control-based approaches
* It is more time-intensive than checklist-driven audits
* It does not easily reduce to simple pass/fail outcomes

These limitations reflect a tradeoff: greater fidelity to actual system behavior and security outcomes, at the cost of simplicity, scalability, and ease of standardization.

## 9. Relationship to Existing Frameworks

This model focuses on how outcomes are evaluated, not how they are enumerated.

SOC 2 and ISO-based frameworks are:

* control-based
* artifact-driven

They emphasize consistency, auditability, and standardized evidence.

In some organizations, particularly earlier-stage companies, these frameworks can serve as an initial forcing function for introducing basic security practices (for example, access control, logging, or vendor management). This can provide useful baseline structure, but does not change the underlying model: assurance is still derived from control definitions and artifacts, rather than direct evaluation of system behavior.

NIST CSF is:

* more outcome-oriented
* closer to how security programs are actually structured
* often used to ensure that major capability areas have been considered

OWASP ASVS is:

* implementation-focused
* used to ensure that security requirements are realized in application code and configuration

This model can be used alongside these approaches. It does not define which outcomes to include or how systems should be built, but provides a way to evaluate whether the intended security outcomes are actually achieved in real systems, rather than inferred from control evidence.

As discussed earlier, applying this model in practice requires a defined set of outcomes to establish sufficient coverage. This model operates at the level of evaluation, not coverage definition.

NIST CSF and OWASP ASVS are used here as representative examples of approaches that reflect how security programs are structured and implemented in practice. They are not intended as an exhaustive or prescriptive set of inputs, and other models or standards may serve similar roles.

## 10. Status

This is an exploratory model, not a standard. It is intended to clarify a different approach to evaluating security outcomes, not to define a complete or finalized framework.
