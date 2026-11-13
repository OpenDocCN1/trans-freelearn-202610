| MEMORY_ONLY_SER | LOW | HIGH | NO | YES |
|---|---|---|---|---|
| MEMORY_ONLY_SER_2 | LOW | HIGH | YES | YES |
| DISK_ONLY | LOW | HIGH | NO | YES |
| DISK_ONLY_2 | LOW | HIGH | YES | YES |
| MEMORY_AND_DISK | HIGH | MEDIUM | NO | PARTIAL |
| MEMORY_AND_DISK_2 | HIGH | MEDIUM | YES | PARTIAL |
| MEMORY_AND_DISK_SER | LOW | HIGH | NO | YES |
| MEMORY_AND_DISK_SER_2 | LOW | HIGH | YES | YES |
| OFF_HEAP | LOW | HIGH | NO | YES |

# List of Storage Level in Persist()

Data is always serialized when it is written to `DISK_ONLY` or `OFF_HEAP` and hence there is no storage level with `_SER` in its name when choosing only disk or off heap. With storage level `MEMORY_ONLY` a situation can arise when the amount of data to be persisted is more than that available in the memory, in such