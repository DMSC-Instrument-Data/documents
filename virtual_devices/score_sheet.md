| Consideration        | Weighting        | Score  | Weighted Score |
| ------------- |:-------------:| -----:| -----:| -----:|
| Technology is free to use | 0.8 | | |
| Technology works cross-platform| 0.8 | | |
| Image creation is scriptable | 0.8 | | |
| Multiple images (5+) can be run simultaneously in a development environment | 0.8 | | |
| Software can create private network of images | 0.8 | | |
| ICS Score (what do they prefer to use) | 0.8 | | |
| DMSC Score (what do they prefer to use) | 0.6 | | |
| PSI Score (what do they prefer to use) | 0.3 | | |
| ISIS Score (what do they prefer to use) | 0.3 | | |

Docker
------
| Consideration        | Weighting        | Score  | Weighted Score |
| ------------- |:-------------:| -----:| -----:| -----:|
| Technology is free to use | 0.8 | 10 | 8 |
| Technology works cross-platform| 0.8 | 10 | 8 |
| Image creation is scriptable | 0.8 | 10 | 8 |
| Multiple images (5+) can be run simultaneously in a development environment | 0.8 | 10 | 8 |
| Software can create private network of images | 0.8 | 10 | 8 |

Vagrant
-------
| Consideration        | Weighting        | Score  | Weighted Score |
| ------------- |:-------------:| -----:| -----:| -----:|
| Technology is free to use | 0.8 | 10 | 8 |
| Technology works cross-platform| 0.8 | 10 | 8 |
| Image creation is scriptable | 0.8 | 9 | 7.2 |
| Multiple images (5+) can be run simultaneously in a development environment | 0.8 | 8 | 6.4 |
| Software can create private network of images | 0.8 | 10 | 8 |

Comments
--------
  - Technologies seem to be aimed at similar problems
  - Scripted creation of Vagrant containers requires additional program called Packer
  - Resource use of complete VMs is higher than for Docker containers
  - For clearly defined things like "a virtual device", Docker seems like the way to go
  - For development environment or other complex software (the entire control system?), virtual machine may be more appropriate (system is closer to "actual computer")
  - Informed ICS about Docker-packaging in integration meeting, seemed interesting for them as well