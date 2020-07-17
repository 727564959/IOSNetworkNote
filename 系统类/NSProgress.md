## NSProgress

NSProgress用来报告当前某个任务的进度, 或者多个任务的进度和这些任务的总进度等。NSProgress可以是一个树状结构, 一个节点进度可以有多个子节点, 每一个子节点只有一个父节点, 每一个子节点都有自己独立的进度体系, 并且子节点完成任务后, 会将完成的数量反馈给父节点。

#### 基本属性

- **totalUnitCount**: 总单元, 用来记载某个任务的总单元数 (可以理解为某个任务正常结束时, 需要完成的任务总量)
- **completedUnitCount**: 已完成单元数量, 记载某个任务执行过程中已经完成的单元数量 (可以理解为, 某一个任务在执行过程中已经完成的任务量)
- **fractionCompleted**: 某个任务已完成单元量占总单元量的比例
- **localizedDescription**: 通过字符串的形式描述当前任务完成度，格式 : 100% completed
- **localizedAdditionalDescription**: 同localizedDescription一样, 用来描述当前任务的完成度，格式: `9 of 10` 

#### 添加子节点

+ (NSProgress *)progressWithTotalUnitCount:(int64_t)unitCount parent:(NSProgress *)parent pendingUnitCount:(int64_t)portionOfParentTotalUnitCount

- -(void)addChild:(NSProgress *)child withPendingUnitCount:(int64_t)inUnitCount





