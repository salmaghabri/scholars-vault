# DEF
## Policies
-  why do I need to do this?
- high-level expectations 
- Identify issue and scope
## Standards
- what are the requirements?
- mandatory and quantifiable
## Procedures
- how do I do it?
- step-by-step process to take
## Guidelines
- General rule
- suggested actions
- recommended procedures
# Examples
Example 1: 
-  **Policy**: Apply security controls on data
-  **Standard**: Authentication, Authorization, Audit (AAA) ==should ==be  enabled in ==any== corporate software 
-  **Procedure**: use the appropriate implementation of the AAA (example of identity <ins>management solutions</ins>: Auth0, Okta) that could fit with the needs of the use case 
-  **Guideline**: ==Recommended step-by-step approach== on how to integrate the platform with Auth0 or Okta to make sure all the users authenticate through that identity management solution
Example 2: 
-  **Policy**: Security measures of the software should be updated and tested regularly to ensure it remains protected from potential threats.
-  **Standard**: Internally developed software projects ==should ==use the corporate static and dynamic software code analysis product 
-  **Procedure**: Every new git commit or git merge request of the internally developed software, the new version of the code should be scanned by the corporate static and dynamic software code analysis product 
-  **Guideline**:==Recommended step-by-step approach== on how to make the scan trigger on a git commit or on a git merge request
# Summary

![[Policies vs Standards vs Procedures vs Guidelines-1.png]]

# Policies Enforcement Tools and techniques
