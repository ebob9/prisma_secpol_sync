# prisma_secpol_sync
Tool to sync Prisma Access Security Policies to Prisma SD-WAN NGFW Security Policy sets.

## Goal
This tool allows both Prisma Access Security policies and Prisma SD-WAN NGFW Security Policy sets to use the same rules automatically.

## How it works
1. The tool connects to the SASE API, and examines the currently commited Prisma Access Policysets. 
2. It examines rules in the Prisma Access or Prisma Access/Remote Networks containers.
3. If the rule is fully supported by Prisma SD-WAN, it syncs the rule to a Prisma SD-WAN Security Policyset named `Prisma Access Policies (Synced)`.
4. If the rule match criteria is supported (app-id, user-id, ip / prefix, zone, etc.), but there is additonal threat protection needed, the rule is synced to the Prisma SD-WAN Security Policyset `Prisma Access Inspection Policies (Synced)`.
5. If the rule has any unsupported match criteria, any available criteria (at least name/description) is synced to the Prisma SD-WAN Security Policyset named `Prisma Access Policies Unsupported (Synced)`, and set to `deny` regardless.

## How to use synced policies
The synced Security Policysets can be used in a Prisma SD-WAN NGFW Security Policyset stack. This allows for these policies to be automatically applied in specified order to any existing Prisma SD-WAN security policies already in place.

`Prisma Access Policies (Synced)` are fully supported by the Prisma SD-WAN engine, and can be used for any traffic.

`Prisma Access Inspection Policies (Synced)` require additonal security inspection, so traffic matching these rules should forward to Prisma Access or another security inspection point that supports these additonal capabilities.

`Prisma Access Policies Unsupported (Synced)` are not supported rules, and are for informational/logging purpouses only. If features are enabled to support the unsupported functions in these rules, these rules will automatically move to one of the other synced policy sets.


