---
layout:     post
title:      CListControl点击事件
subtitle:   MFC
date:       2019-10-15
author:     YiMiTuMi
header-img: 
catalog: true
tags:
    - MFC
---

# CListControl中点击Checkbox事件（例）

在**ListControl**中，要点击其中的行，或者其中的**Checkbox**时，要触发点击事件。

在xxx.h中添加：

	afx_msg void OnClickList(NMHDR* pNMHDR, LRESULT* pResult);
	CListCtrl m_list; //关联列表框的控件变量

在xxx.cpp中添加：

当前的**LVN_ITEMCHANGED**产生消息的时间为当某个项已经发生变化时

	ON_NOTIFY(LVN_ITEMCHANGED, IDC_xxx_列表框, &xxx::OnClickList)

	void  xxx::OnClickList(NMHDR* pNMHDR, LRESULT* pResult)
	{
		DWORD dwPos = GetMessagePos();
		CPoint point(LOWORD(dwPos), HIWORD(dwPos));

		m_list.ScreenToClient(&point);

		LVHITTESTINFO lvinfo;
		lvinfo.pt = point;
		lvinfo.flags = LVHT_ABOVE;

		UINT nFlag;
		int nItem = m_list.HitTest(point, &nFlag); //获取点击的行号

		//判断是否点击的Checkbox
		if (nFlag == LVHT_ONITEMSTATEICON)
		{
			if (m_lists.GetCheck(nItem) == FALSE) 
			{
				//点击未选中的时
			}
			else if (m_list.GetCheck(nItem) == TRUE) 
			{
				//点击选中的时
			}
		}
	}

## 波斯菊 -- 永远快乐
