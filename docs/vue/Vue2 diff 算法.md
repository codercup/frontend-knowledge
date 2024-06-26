# Vue2 diff 算法

Vue 2 的虚拟 DOM (Virtual DOM) diff 算法是用于高效地更新视图的关键部分。这个算法在 Vue 的源码中主要位于 `src/core/vdom/patch.js` 文件中，它负责比较新旧虚拟节点 (vnodes)，并决定如何最小化 DOM 操作以达到更新界面的目的。Vue 2 的 diff 算法基于 Snabbdom 的启发，但进行了优化以适应 Vue 的特性。

以下是 Vue 2 diff 算法的一些关键点：

1. **键值对匹配**：
   - 如果子节点有 `key` 属性，Vue 会尝试通过键值对进行精确匹配，这样可以更快地找到对应的旧节点，减少不必要的 DOM 操作。
   - 没有 `key` 的节点则按照它们在列表中的顺序进行比较。
2. **同层节点比较**：
   - 首先，diff 算法会尝试找到新旧节点列表中的相同节点，通过 `key` 进行匹配或按顺序匹配。
   - 使用 `sameVnode()` 函数检查两个节点是否是同一个节点，考虑 `tag`、`data` 和 `children`。
3. **节点移动与插入**：
   - 如果找到了相同节点，Vue 会尝试重用旧的 DOM 节点，而不是创建新的。
   - 如果新节点的位置发生了变化，Vue 会计算出最小移动距离，尽量减少 DOM 的移动操作。
4. **节点的添加与删除**：
   - 对于新增的节点，Vue 会在适当的位置插入它们。
   - 对于被移除的节点，Vue 会将其从 DOM 中删除。
5. **递归处理子节点**：
   - 如果节点是元素节点，diff 算法会递归处理其子节点。
   - 如果节点是文本节点，直接进行文本内容的比较和更新。
6. **优化策略**：
   - Vue 有一个 `patchFlag` 机制，用来标记某些特定的变更，比如文本内容或属性的变更，这样在比较时可以直接跳过某些步骤，提高性能。

Vue 2 的 diff 算法复杂度在平均情况下接近 O(n)，但在最坏的情况下可以达到 O(n^2)，这通常发生在大量无键节点的列表中。Vue 3 中对 diff 算法进行了改进，引入了更多的优化策略，如分块处理和预排序，以进一步提高性能。
