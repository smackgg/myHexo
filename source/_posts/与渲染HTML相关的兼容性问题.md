title: 与渲染HTML相关的兼容性问题
date: 2015-08-12 18:15:14
tags: 兼容性
categories: 与渲染HTML相关的兼容性问题
keywords: html渲染兼容性, 前端兼容性
---
内容如题
<!-- more -->

> - 1、注释内容含中横线（-）在 Firefox 中可能会使中间内容丢失。
>> 解决办法：按标准推荐的方法写注释标签

> - 2、各浏览器对于字符编码别名支持的宽泛程度有差异，当指定了浏览器无法识别的字符编码别名时，浏览器会以确定编码的优先级顺序采用设置的更低优先级的字符编码，以此类推。
而 Chrome Safari Opera 中对字符编码别名有着比其他浏览器更宽泛的支持。
>> 解决办法：首先，对于动态页面必须确保 HTTP "Content-Type" 头字段的 "charset" 参数与页面自身编码相符，且务必在页面的 META 元素中也声明相符的字符编码信息。对于静态页面，必须保证页面中 META 元素声明中 "http-equiv" 为 "Content-Type" 对应的值中的 "charset" 的值与页面自身编码相符。
其次，在设置字符编码别名时，最好使用最通用的、各浏览器均可识别的编码别名。

> - 3、对于 URI 中非 ASCII 字符，并非所有浏览器都是按照 HTML 4.01 规范中的建议实现的，而且不同浏览器在处理不同形式的 URI 时，表现也有差异。
>> 解决办法：当 URI 中含有非 ASCII 字符时，不要依赖浏览器对 URI 的编码方式，以避免产生差异

> - 4、如果在 DTD 之前加入注释或其他内容，在某些浏览器中该 DTD 将无法被识别。
>> 解决办法：声明 DTD 时，确保 DTD 之前没有其他字符，即便有，也只能是空格符、换行符和制表符。如将 DTD 放在 HTML 文档的第一行。

> - 5、Chrome 和 Safari 中标签紧密相邻的行内元素在折行显示时存在错误。
>> 解决办法：避免出现紧密连接的内联元素标签，可以在每个标记之间加入空格或者换行符来避免这个问题。

> - 6、通过 META 元素可以控制页面定时跳转，对于 http-equiv 属性为 refresh 时对应的 content 属性的值中，跳转时间与跳转 URL 之间需要分隔符分开，如果使用非 ';' 的分隔符时，在某些浏览器下将不能达到期望的效果。
>> 解决办法：
>> - 参照 W3C 的建议，使用服务端进行页面跳转。
>> - 使用合法的，所有浏览器均支持的写法：

> - 7、IE6/7 及 IE8 混杂模式(Q) 会忽略同一行内 OBJECT、IFRAME 元素之后的空白符。
>> 解决办法：若不希望出现空格，可以将 IFRAME OBJECT 元素设置为块级元素。


> - 8、IE6 IE7 IE8(Q) 中当 OBJECT 元素之前的行内文本由 "&nbsp;" 构成，且 "&nbsp;" 宽度之和小于容器宽度时，OBJECT 元素不发生折行；而 "&nbsp;" 宽度之和超过容器时，OBJECT 元素会折行显示。而在 IE8(S) Opera 中，出现上述情况时，OBJECT 元素始终不会发生折行。
>> 解决办法：
合理的设置容器及 OBJECT 元素的宽度。若需要 OBJECT 元素不发生折行，则为容器设置 "white-space:nowrap" ；
若需要 OBJECT 元素折行，则在 OBJECT 元素之前加入明确的换行符 "<br />" 。

> - 9、Chrome 和 Safari 中 BR 元素前的空白符不会被忽略，多余的空白符将被压缩为一个空白符并渲染到 BR 元素之前的行中。
>> 解决办法：删除 BR 元素之前多余的空白符。

> - 10、单元格的 colspan 属性在 IE 中可能影响 TABLE 元素的自动布局。
>> 解决办法：
>> - 设置 TABLE 的 'table-layout' 特性值为 fixed，使用固定布局的表格元素可避免此问题。
>> - 单元格所跨过的列的宽度都设置成 auto。

> - 11、在 Firefox 中，TABLE 元素 'width' 属性的百分比值大于 100% 时，Firefox 会按 100% 处理；如果是 CSS 的 'width' 特性，则不会这么处理。
>> 解决办法：给 TABLE 元素设置宽度的时候，不要使用 HTML 属性 'width'，请使用 CSS 特性 'width'。

> - 12、在 IE 中，不仅 TD 和 TH 元素，其他一些元素也拥有 noWrap 属性。
>> 解决办法：nowrap 属性是被废弃的属性，使用 CSS 规则 white-space:nowrap 代替这个属性。

> - 13、IE6 IE7 IE8(Q) 对 COL 和 COLGROUP 元素的属性及部分 CSS 特性支持较好，而IE8(S) Firefox Chrome Safari 不再支持 COL 及 COLGROUP 元素的部分属性及为其设定的 CSS 特性。
>> 解决办法：当使用 COLGROUP COL 时需注意各浏览器对属性及 CSS 样式的设置，避免造成兼容性问题。

> - 14、若表格设定了大于零的水平单元格间隙 (即 cellspacing 属性)，且其内单元格存在过度设定的 colspan 属性，也就是单元格横跨的列数的设定值多余实际存在的单元格的个数，这时各浏览器对表格的渲染效果存在较大差异。
>> 解决办法：在进行表格布局时，务必精准设定 colspan 属性的值

> - 15、BASE 元素出现在 BODY 中，且定义了 target 属性，各浏览器表现不一致。
>> 解决办法：不要在 HEAD 元素之外定义 BASE 元素，保证各浏览器兼容。

> - 16、通常情况下，IE 系列浏览器通过 ActiveX 插件使用 OBJECT 元素引入 Flash，而其他浏览器则是通过相应的 NPAPI 插件使用 EMBED 元素。这造成了各浏览器中插入 Flash 的方式的差异。
>> 解决办法：
>> - 若不考虑 W3C 校验，可统一使用 EMBED 元素嵌入 Flash，这样可以避免因参数不统一导致的兼容性问题。
>> - 若需要考虑 W3C 校验（swfobject Markup Validation Service），则可使用第三种单独使用 OBJECT 与 PARAM 元素的方式。
>> - 若必须使用 OBJECT 嵌套 EMBED 元素这种混合方式，则要保证 Flash 文件 URL、为 Flash 传递的参数、宽度、高度、wmode 等参数保持统一。
>> - 可以使用开源的 SWFObject 引入 Flash。（请参见：swfobject）

> - 17、该问题将造成局部的布局混乱。
>> 解决办法：为了防止这种无 "src" 的 IMG 元素对页面产生布局影响，需要设置这种 IMG 的 ‘display’ 特性为 'none'。

> - 18、alt 属性在 IE6 IE7 IE8(Q) 下具有双重意义。在给 IMG、AREA、INPUT[type=image] 元素设置的 alt 属性值不但可以作为该元素的替代文字，在该元素没有指定 title 属性时，还可以作为提示信息（tooltip）被显示出来。
>> 解决办法：
>> - 若用户需要显示提示框，则指定 title 属性；
>> - 若用户不需要显示提示框，则指定 title=""。

> - 19、PNG24 格式图片可以携带 Alpha 半透明通道，以便呈现从透明到不透明间过渡色彩效果，但是 IE6 不支持这种格式原有的透明特性。
>> 解决办法：
- 使用 IE 专有滤镜 AlphaImageLoader Filter 来修复 IE6 透明通道问题
- 或者使用脚本批量处理方式：

	function fixpng24(){
	    var arVersion = navigator.appVersion.split("MSIE");
	    var version = parseFloat(arVersion[1]);
	    if ((version >= 5.5) && (document.body.filters)){
	       for(var i=0; i<document.images.length; i++){
	          var img = document.images[i];
	          if (img.src.toLowerCase().slice(-3) == "png"){
	             var imgID = (img.id) ? "id='" + img.id + "' " : "";
	             var imgClass = (img.className) ? "class='" + img.className + "' " : "";
	             var imgTitle = (img.title) ? "title='" + img.title + "' " : "title='" + img.alt + "' ";
	             var imgStyle = "display:inline-block;" + img.style.cssText ;
	             if (img.align == "left") imgStyle = "float:left;" + imgStyle;
	             if (img.align == "right") imgStyle = "float:right;" + imgStyle;
	             if (img.parentElement.href) imgStyle = "cursor:pointer;" + imgStyle;
	             var strNewHTML = "<span " + imgID + imgClass + imgTitle
	             + " style=\"width:" + img.width + "px; height:" + img.height + "px;" + imgStyle
	             + "filter:progid:DXImageTransform.Microsoft.AlphaImageLoader"
	             + "(src='" + img.src + "', sizingMethod='scale');\"></span>";
	             img.outerHTML = strNewHTML;
	             i--;
	          }
	       }
	    }
	}


> - 20、当给一个现有的 IMG 元素重设其 "src" 为其当前的 "src" 时，只有 IE6 会重现载入该图片，其他浏览器则不会。
>> 解决办法：如果需要重复设置相同的 src 值时，均触发 IMG 的 onload 事件，或者需要每次均从服务器端下载图片数据的时候，可以采用图片地址后加上随机数或当前时间戳参数的手段，避免内容被缓存。

> - 21、IE Chrome Safari 支持通过为 OBJECT 元素设置 classid 引入 Windows 下的 Media Player 或 Flash 插件。而 Windows 版的 Firefox 与 Opera 无法支持这种形式引入这些插件。
>> 解决办法：由于某些浏览器原生无法支持 OBJECT 元素使用 classid 属性引入 Media Player 插件，所以为保证最大的兼容性，应避免使用此方式在页面中播放媒体文件。
可以考虑使用 Flash，或者合理的利用 IE 对 Media Player 的支持及其其他浏览器对 HTML5 的新标签 "VIDEO" 与 "AUDIO" 的支持在不同浏览器中达到相类似的效果。

> - 22、Firefox Opera 中 OBJECT 元素的默认尺寸为不可视；而 IE 中，OBJECT 默认尺寸为 16 x 16 像素；在 Chrome 和 Safari 下的默认尺寸为 300 x 150 像素。
>> 解决办法：OBJECT 元素为替换元素，应为 OBJECT 元素设置一个明确的宽度和高度。

> - 23、IE Opera 中，IMG 元素通过其 usemap 属性可以与 MAP 元素的 id 属性及 name 属性关联，而 Firefox Chrome Safari 中仅限于 MAP 元素的 name 属性。
>> 解决办法：若需要 IMG 元素与 MAP 元素相关联，注意通过 IMG 元素的 usemap 属性关联的 MAP 元素的 name 属性的值。

> - 24、MAP 元素会影响指向 MAP 的元素，其父元素 A 元素的默认链接行为。
IE6 IE7 IE8 中，产生自 MAP 元素的事件冒泡路径，不依赖 MAP 本身所处的元素嵌套规则，他会执行到引向 MAP 的元素嵌套结构中。
>> 解决办法：
>> - 注意 MAP 标记与其他标记的嵌套关系，IE6 IE7 IE8 中的 MAP 冒泡机制是根据指向 MAP 标记的标记位置决定的。如果期望所有浏览器冒泡路径基本一致，可以将 MAP 标记放在引用 MAP 的标记的兄弟级别。
>> - 注意 A 标记的默认导航行为触发限制，即使标签嵌套一致，点击事件冒泡路颈 A 标记，也不会在 IE6 IE7 IE8 Firefox 中触发浏览器默认行为产生导航。可以利用 A 标记也会执行点击事件冒泡的特性，使用 A 标记的 "click" 事件代替 "href" 属性执行页面跳转工作。

> - 25、CENTER 元素在 IE6 IE7 IE8(Q) 中会使自身也居中对齐，除了上述浏览器，在 IE(S) 与 Opera(S) 中 CENTER 元素还会使其内表格中的单元格内文本居中对齐。
>> 解决办法：避免使用 CENTER 标签，使用 CSS 的 'text-align' 特性来代替。

> - 26、Firefox Chrome Safari 会将 DIV H1~H6 P 元素的 align="middle" 解释为 align="center"，从而使这些元素能够居中对齐。
Firefox 混杂模式会将 TABLE 元素 align="middle" 解释为 align="center"，使 TABLE 元素居中对齐。
IE6 IE7 Chrome Safari Opera 及 IE8 Firefox 的混杂模式下，均将 TD TH 元素的 align="middle" 理解为 align="center"。
>> 解决办法：align="middle" 仅在 IMG、OBJECT、APPLET 元素上的 align 属性中是合法值，对于其他元素的 align 属性均为非法。各浏览器在上述三个元素之外的元素上遇到 align="middle" 均按照自己的理解方式解释。同时除单元格元素的 align 属性之外，其他的 align 属性均被 W3C 官方废弃（Deprecated.），所以应避免使用此属性。
