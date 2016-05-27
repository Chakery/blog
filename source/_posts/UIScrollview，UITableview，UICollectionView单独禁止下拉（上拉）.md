---
title: UIScrollview，UITableview，UICollectionView单独禁止下拉（上拉）
date: 2016-05-27
---

UIScrollview，UITableview，UICollectionView 完全禁止弹簧效果，只需要设置`bounces = false`即可。
如果需要单独禁止上拉，或者下拉。
```
func scrollViewDidScroll(scrollView: UIScrollView) {
    // 禁止下拉
    if scrollView.contentOffset.y <= 0 {
        scrollView.contentOffset.y = 0
    }
    // 禁止上拉
    if scrollView.contentOffset.y >= scrollView.contentSize.height - scrollView.bounds.size.height {
        scrollView.contentOffset.y = scrollView.contentSize.height - scrollView.bounds.size.height
    }
}
```
