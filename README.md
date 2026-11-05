## 问题
部分发票（例如滴滴出行电子发票）的 MediaBox 是整张 A4（`[0 0 595 842]`），
但 CropBox 只框住上半部分（`[0 433 610 864]`）。
`Merge2` / `Merge4` 按 `PdfImportedPage` 的 MediaBox 尺寸计算缩放比例，
导致下半页空白也参与计算，合并后发票显示特别小。

## 修改
- 新增 `ApplyVisibleBox`：取 CropBox 与 MediaBox 的交集作为可见区域，并设置为导入页的 BBox，裁掉不可见部分。
- `Merge2` / `Merge4` 按可见区域计算缩放比例，平移时扣除可见区域的左下角坐标，保证居中。
- CropBox 与 MediaBox 相同的普通发票，计算结果与原来一致。

## 测试
用上述滴滴发票测试：
- 4 合 1：缩放比例 0.31 → 0.63
- 2 合 1：缩放比例 0.45 → 0.93

已核对输出 PDF 中每张发票的变换矩阵和 BBox。

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## 界面预览
![ui](https://user-images.githubusercontent.com/1094462/50446646-0c9b0e00-0951-11e9-89d4-b8306d27ac07.png)
