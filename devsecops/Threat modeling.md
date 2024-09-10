- happens in all phases but best before coding
- begginer friendly, not for complex systems tho
# goal
- ensure CIA (cofidentiality, integrity, availability)
- or extended CIA (CIA + non repudiation+ authentication+ authorization )
# Threats
- [[Spoofing]]
- [[Tampering]]
- [[Repudiation]]
- [[Information Disclosure]]
- [[Denial of Service]]
- [[Elevation of Privilege]]
# Steps
## 1. Diagramming:  
[[Data Flow Diagram (DFD)]] 
## 2. Threat Enumeration : Determining the criticality of all components
### Some rules ( There is no general rule)
1. external := 0; no action / control on it (users)
2. first interaction with external boundary:= 1 
3. external and not high importance := 2 or 3 (logging process): importance == if it is lost => big damage #medium_risk
4. medium size risk := 4 or 5  #medium_risk
5. any change on this process can have a business impact (hit the disk): 6 or 7 (databases, web pages) #high_risk
6. critical elemnts :8 or 9 (database files) #high_risk 
### Example
![[Threat modeling-1.png]]
### Applying STRIDE
![[Threat modeling-2.png]]

![[Threat modeling-3.png]]
## 3. Mitigation 
## 4. Validation
