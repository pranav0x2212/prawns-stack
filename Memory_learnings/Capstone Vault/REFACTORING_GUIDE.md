# TikZ Diagram Refactoring Guide

## Overview
The ROB diagram has been refactored to use **relative positioning** instead of hardcoded coordinates. This makes the diagram significantly easier to maintain and modify.

## Key Improvements

### 1. **Positioning Library** (`positioning`)
Instead of absolute coordinates like `(x, y)`, nodes are now positioned relative to each other.

**Before:**
```latex
\node[BLKS=CliD/CliF] (seqgen) at (7.5, 0) {...};
\node[BLKS=PurD/PurF] (taginst) at (13.0, 0) {...};
```

**After:**
```latex
\node[BLKS=CliD/CliF, right=2cm of rcp] (seqgen) {...};
\node[BLKS=PurD/PurF, right=2.2cm of seqgen] (taginst) {...};
```

**Positioning directives:**
- `right=2cm of nodeA` - Place 2cm to the right of nodeA
- `below=3.5cm of nodeB` - Place 3.5cm below nodeB
- `above=1.2cm of nodeC` - Place 1.2cm above nodeC
- `left=...` - Place to the left

### 2. **Matrix of Nodes** for Queue Structure
The queue structure (multiple AXID columns with multiple slots) uses a `matrix` for clean organization.

**Structure:**
```latex
\matrix[
  matrix of nodes,
  nodes={anchor=center},
  column sep=3.8cm,  % Space between columns
  row sep=0.8cm,    % Space between rows
  below=4.5cm of seqgen,
  right=1.8cm of seqgen,
  name=queue-matrix
] {
  % Row 1: Headers (AXID=0, AXID=1, AXID=N)
  % Row 2-5: Slot contents
};
```

**Benefits:**
- Regular grid layout is automatic
- Easy to add/remove queue columns
- Adjusting `column sep` and `row sep` adjusts spacing globally
- Each cell can have different node styles

### 3. **Fit Nodes** for Logical Grouping
The ROB boundary box now uses a `fit` node that automatically adjusts to contain all internal elements.

**Before:**
```latex
\draw[CliD, dashed] (4.0, 3.0) rectangle (37.8,-7.2);
```

**After:**
```latex
\node[
  fit=(seqgen) (taginst) (queue-matrix) (fill) (retire),
  inner sep=0.8cm,
  rounded corners=8pt,
  draw=CliD,
  dashed
] (rob-boundary) {};
```

**Benefits:**
- Boundary automatically adjusts when elements move
- Adding new elements: just add to the `fit=` list
- Consistent spacing with `inner sep`

## Conversion Guide: Absolute → Relative

### Basic Node Positioning
```latex
% Old: absolute position
\node[STYLE] (name) at (10, 5) {text};

% New: relative position
\node[STYLE, right=3cm of other-node] (name) {text};
```

### Multiple Nodes in a Row
```latex
% Old: calculate each x-coordinate
\node[...] (node1) at (2.0, 0) {...};
\node[...] (node2) at (7.5, 0) {...};
\node[...] (node3) at (13.0, 0) {...};

% New: chain them relative to each other
\node[BLKS=...] (node1) {...};
\node[BLKS=..., right=2cm of node1] (node2) {...};
\node[BLKS=..., right=2.2cm of node2] (node3) {...};
```

### Grid Structure (Queues, Tables, etc.)
```latex
% Old: manual coordinate calculation for each cell
\node[SLOT=...] (q0s0) at (19.5, -3.1) {seq=0};
\node[SLOT=...] (q0s1) at (19.5, -3.9) {seq=1};
% ... many more nodes ...

% New: use matrix
\matrix[
  matrix of nodes,
  column sep=3.8cm,
  row sep=0.8cm,
  name=queue-matrix
] {
  HEADER0 & HEADER1 & HEADERN \\
  q0s0    & q1s0    & qNs0    \\
  q0s1    & q1s1    & ...     \\
  % ... etc
};
```

### Grouping with Fit
```latex
% Old: manually drawn rectangle with fixed coordinates
\draw[dashed] (4.0, 3.0) rectangle (37.8, -7.2);

% New: fit node that adapts automatically
\node[fit=(element1) (element2) (element3), 
       draw, dashed, inner sep=0.8cm] (group) {};
```

## Maintainability Examples

### Adjusting Width Between Issue Path Blocks
To increase spacing between seqgen and taginst:

```latex
% Simple change:
\node[BLKS=PurD/PurF, right=3cm of seqgen] (taginst) {...};
%                            ^^^ was 2.2cm
```

### Adding a New Queue Column
To add AXID=M column, just modify the matrix:

```latex
\matrix[...] {
  AXID=0 & AXID=1 & AXID=M & AXID=N \\
  ...    & ...    & ...    & ...    \\
};
```

The ROB boundary and all buses automatically adjust!

### Changing Queue Slot Height
```latex
\matrix[
  row sep=1.0cm    % was 0.8cm - instant global adjustment
] {...};
```

## Accessing Matrix Nodes

In the refactored diagram, matrix cells are referenced as:
```
queue-matrix-ROW-COL

Examples:
queue-matrix-1-1  % Row 1, Col 1 (first header)
queue-matrix-2-1  % Row 2, Col 1 (first queue column, first slot)
queue-matrix-3-2  % Row 3, Col 2 (second queue column, second slot)
```

## Connecting Elements

### Buses and Arrows
Instead of connecting to fixed coordinates:

```latex
% Old: hardcoded coordinates
\draw[BUS=PurD] (24.5, -7.6) -- (24.5, -6.8);

% New: relative to nodes
\draw[BUS=PurD] (fill.north) -- ($(fill.north) + (0, 0.8)$);
```

Using `$...$` for coordinate calculations:
```latex
% Calculate midpoint between two nodes
($(node1.east)!0.5!(node2.west)$)

% Offset from a node
($(node.center) + (1cm, 0)$)

% Percentage of path between two points
($(start)!0.6!(end)$)  % 60% of the way from start to end
```

## Best Practices for Future Diagrams

1. **Define spacing as variables** at the top:
   ```latex
   \pgfset{
     block sep=2cm,      % Space between blocks
     queue width=3.8cm,  % Queue column width
     device below=4.5cm  % Vertical distance to sub-elements
   }
   ```

2. **Use meaningful node names:**
   - `seqgen` not `a` or `node1`
   - `queue-matrix` not `m1`
   - `retire` not `ret_log`

3. **Group related elements:**
   ```latex
   % Issue path group
   \node[...] (issue-path) [fit=(rcp) (seqgen) (taginst)] {};
   
   % Return path group  
   \node[...] (return-path) [fit=(dram) (fill) (retire)] {};
   ```

4. **Document node hierarchy:**
   ```latex
   % Top-level regions
   % - Issue path (left)
   % - Queues (center-top)
   % - Fill logic (center-bottom)
   % - Retirement (right)
   % - External devices (edges)
   ```

5. **Use symbolic spacing:**
   - Small gaps: `0.4cm` to `0.6cm`
   - Medium gaps: `1.2cm` to `2cm`
   - Large gaps: `3cm` to `5cm`
   - Consistency: use the same value for similar purposes

## Conversion Checklist

- [ ] Add `positioning` and `matrix` to `usetikzlibrary`
- [ ] Identify logical groups of nodes
- [ ] Replace issue path with sequential `right=` positioning
- [ ] Convert queue structure to `matrix` if applicable
- [ ] Replace block and bus coordinates with relative positioning
- [ ] Replace boundary boxes with `fit` nodes
- [ ] Test compilation and visual alignment
- [ ] Document node naming convention in code comments
- [ ] Update maintenance notes for future changes

## Common Pitfalls

1. **Forgetting node anchors** - When positioning relative to nodes, be specific:
   - `right=1cm of node.east` (not just `right=1cm of node`)
   - `below=1cm of node.north` (not `below=1cm of node`)

2. **Circular dependencies** - Don't reference a node before it's defined:
   ```latex
   % WRONG:
   \node[right=2cm of nodeB] (nodeA) {};
   \node (nodeB) {};
   
   % CORRECT:
   \node (nodeB) {};
   \node[right=2cm of nodeB] (nodeA) {};
   ```

3. **Matrix cell coordinates** - Use proper syntax:
   ```latex
   queue-matrix-2-1  % correct
   queue_matrix_2_1  % WRONG
   (queue-matrix)(2-1) % WRONG
   ```

## References

- TikZ Positioning Library: `texdoc tikz` → "Positioning Nodes Relative to Others"
- TikZ Matrix Library: `texdoc tikz` → "Matrix Positioning Nodes"
- Fit Node: `texdoc tikz` → "Fit Library"

## Next Steps

Apply this refactoring approach to:
- Scheduler diagrams
- Memory hierarchy diagrams  
- Datapath diagrams
- Control flow diagrams

Each will become easier to maintain and modify!
