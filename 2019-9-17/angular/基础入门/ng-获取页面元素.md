## 获取组件元素

引入viewchild elementref

	import { Component, OnInit, ViewChild, ElementRef } from '@angular/core';

声明变量

	@ViewChild('descList') descList: ElementRef;

note：descList为组件中‘#descList’标识的元素

	<div class="row hx-desc scroll-list" #descList>