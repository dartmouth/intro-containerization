# What are containers?

## History

- In the beginning we had physical servers
- Then we got Virtual Machines (VMs)
  - Virtualize the OS
- Now containers
  - virtualize an application
- Next is functions
  - E.g. AWS Lambda, abstract code run space

## How do you virtualize an application?

- C-groups and namespaces
  - Built into Linux kernel
  - Gives a process it's own sandbox
- Since Linux standardizes at the Kernel level, different OSes can be presented in a container

## Analogy

- A server is a house
  - Own electrical, water, sewage
  - Family owns the house with a mortgage
- A container is an apartment
  - Electrical, water, and sewage is shared, but they have their own bathroom, kitchen, etc.
  - Container is a temporary occupant

Next: [Why do we care?](2-why-do-we-care.md)
