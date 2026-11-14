# CCSA105 – Policy Layer Assessment

**Student:** Morba Chinonso  
**Completion Date:** 14/11/2026

---

# Component

## Supporting Documentation

Screenshots and supporting evidence for each task are included in the assessment report. Please refer to the report for SmartConsole screenshots, policy configurations, layer settings, rulebase views, logs, and other visual evidence referenced in this assessment.

---

## Task 1: Explore the Default Policy and Layer

### Default Access Control Policy and Ordered Layer

| Item | Name | Summary |
|------|------|---------|
| Default Access Control Policy | Standard | Built-in Access Control policy package provided by Check Point. |
| Default Ordered Layer | Network | System-created Ordered Layer used to evaluate network traffic. |

### Default Ordered Layer Settings

#### General
- Firewall enabled
- Applications & URL Filtering disabled
- Content Awareness disabled
- Mobile Access disabled
- Sharing disabled
- Applied to Standard policy package

#### Advanced
- Implicit Cleanup Action: Drop
- Proxy Configuration: Disabled
- Dynamic Layer: Disabled

#### Permissions
- Super User
- Read Write All

### Cleanup Rule

| Item | Observation |
|--------|-------------|
| Policy | Standard |
| Rule Name | Cleanup rule |
| Action | Drop |
| Track | Log |

---

## Task 2: Policy Package and Network Layer

Created a new policy package named **Orion-Layers** with Access Control enabled.

---

## Task 3: Second Ordered Layer

Created a second Ordered Layer named **Service-Control** and placed it below **Network**.

### Service-Control Layer Settings

#### General
- Firewall enabled
- Sharing disabled

#### Advanced
- Implicit Cleanup Action: Drop
- Proxy Configuration: Disabled
- Dynamic Layer: Disabled

#### Permissions
- Super User
- Read Write All

---

## Task 4: Install, Test and Fix

> **Note:** Task 4 could not be completed because no Security Gateway was available for policy installation. While the Orion-Layers policy package and layers were created and configured in SmartConsole, policy installation requires a reachable gateway target. As a result, tests T1–T5 could not be performed and the required logs could not be collected.

---

## Task 5: Inline Layer

Created Inline Layer **DMZ-Policy**.

### DMZ-Policy Settings

#### General
- Sharing Enabled

#### Advanced
- Implicit Cleanup Action: Drop
- Proxy Configuration Disabled
- Dynamic Layer Disabled

#### Permissions
- Super User
- Read Write All

---

## Task 6: Verify the Inline Layer

> **Note:** Task 6 could not be completed because no Security Gateway was available for policy installation. Policy installation, testing (T6–T9), and log verification require an operational Check Point Gateway or Gateway Cluster.

---

## Task 7: Layer Order and Sharing

### Layer Order
- Verified layer order changes.
- Restored original layer sequence.

### Sharing Explanation

The **Sharing: Enabled** setting allows the **DMZ-Policy** layer to be reused across multiple policy packages. Changes made to the shared layer are automatically reflected wherever the layer is used, improving consistency and reducing administrative effort.

### Permission Profile Research

A Permission Profile can be configured so that an administrator has Read/Write access only to the DMZ-Policy layer while other layers remain read-only or inaccessible. This supports role-based management by restricting administrators to the areas they are responsible for and reducing the risk of unauthorized changes.

---

# Part B: Theory

## Section B1

### Q1. Which statement best describes a policy layer?
**Answer:** A set of rules that divides a policy into smaller, more manageable sections.

### Q2. What are the names of the default Access Control policy and its default Ordered Layer?
**Answer:** Standard and Network.

### Q3. A packet matches an Accept rule in the first Ordered Layer. What does the gateway do next?
**Answer:** It continues checking the next Ordered Layer.

### Q4. Which type of layer is attached to a specific rule as a sub-policy?
**Answer:** Inline Layer.

### Q5. What is the recommended practice at the end of each Ordered Layer?
**Answer:** Add an explicit Cleanup rule with the same Action as the Implicit Cleanup Rule.

## Section B2

### Q6
#### Define a policy layer and state why layers are useful in a large network. Name two benefits of using them.
A policy layer in Check Point is a separate set of security rules within a policy package that is evaluated independently. Layers are useful in large networks because they divide complex policies into manageable sections. Two benefits are better organization of rules for easier administration and improved scalability, allowing policies to be managed and updated more efficiently.

### Q7
#### Compare Ordered Layers and Inline Layers. For each, say how it is evaluated and give one use case.
An Ordered Layer is evaluated as part of the policy package sequence, and traffic that matches an Accept rule continues to the next Ordered Layer for further inspection. It is commonly used to organize large rulebases into sections such as Network Access and Application Control. An Inline Layer is evaluated only when traffic matches the rule to which the layer is attached. It is commonly used to create detailed sub-policies for specific traffic, such as VPN users, servers, or a particular department.

### Q8
#### Describe the order of enforcement for Ordered Layers. Include what happens when a rule action is Drop, when it is Accept, and when no rule matches.
Ordered Layers are evaluated sequentially from top to bottom. If a packet matches a Drop rule, the gateway immediately blocks the traffic and stops processing. If a packet matches an Accept rule, the gateway continues checking the remaining Ordered Layers. If no rule matches in a layer, the Cleanup Rule or Implicit Cleanup Rule is applied, which normally drops the traffic.

### Q9
#### Name the three groups of settings you configure for a layer. Give one example of a setting from each group.
The three groups of settings that can be configured for a layer are General, Advanced, and Permissions. The General group is used for basic configuration, such as enabling the Firewall blade. The Advanced group controls how the layer behaves, such as layer usage and processing options. The Permissions group controls administrator access, for example allowing selected administrators to edit the layer.

### Q10
#### Explain what Sharing does for a layer. State one guideline from the module about sharing or reusing layers.
Sharing allows a layer to be used by multiple policies or rules instead of existing in only one policy package. This reduces duplicate rule creation and helps maintain consistent security policies across the environment. A recommended guideline is to share layers only when the same policy needs to be reused in multiple locations and to avoid unnecessary shared layers that can complicate management.

## Section B3

### Q11
#### A policy has two Ordered Layers. A connection is accepted by a rule in the first layer. The second layer has no rule that matches the connection and no explicit Cleanup rule, and its Implicit Cleanup Action is Drop. What happens to the connection? Explain how you would confirm this, how you would fix it if the connection should be allowed, and why an explicit Cleanup rule is recommended.
The connection is dropped. An Accept in the first Ordered Layer only allows the traffic to continue to the next layer for further evaluation. Because no rule matches in the second layer and its Implicit Cleanup Action is Drop, the gateway blocks the traffic. I would confirm this by checking the second layer's rules and reviewing the logs in SmartConsole. To allow the connection, I would add or modify an Accept rule in the second layer, then publish and install the policy. An explicit Cleanup rule is recommended because it clearly shows how unmatched traffic is handled and provides useful logging for troubleshooting.

### Q12
#### A company policy has grown to hundreds of rules. Administrators find it slow to review and are afraid to change it. Propose a layer design using Ordered and Inline Layers. Explain how your design improves manageability, role-based administration and performance.
I would divide the policy into several Ordered Layers such as Network Access, Server Access, Application Control, and Threat Prevention. Within those layers, I would use Inline Layers for specialized policies such as VPN users or departmental access requirements. This design improves manageability by organizing rules into logical sections, improves role-based administration by allowing different teams to manage specific layers, and improves efficiency because Inline Layers are evaluated only when their parent rule is matched. The result is a simpler, more scalable, and easier-to-maintain security policy.
