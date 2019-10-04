# Мои заготовки и наработки


**jQuery**
```HTML
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
```

**Мобильная версия**
```HTML
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

**Vue js - простое подключение**
```HTML
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```


## Паралакс элементов при склолинге
```HTML
<div class="parent">
	<i class="paralax el1" data-k="0.8"></i>
</div>
```
```SASS
.parent
	position: relative

.paralax
	display: block
	position: absolute
	background-position: center
	background-size: 100% 100%
	background-repeat: no-repeat
	transition: 1s

.el1
	width: 100px
	height: 100px
	background-image: url('../img/img.png')
	top: calc(50% - 0px)
	left: calc(50% - 0px)
	z-index: 2
```
```javascript
let screenH	= $(window).height();
$(window).scroll(function() {
	var scrollTop	= $(window).scrollTop();
	var scrollB		= scrollTop + screenH;

	$('.paralax').each(function() {
		var el			= $(this),
			elK			= +el.attr('data-k'),
			// elTop	= +el.attr('data-top'),
			parent		= el.parent(),
			parentTop	= parent.offset().top,
			parentH		= parent.outerHeight(),
			marginT		= 0;

		if (
				(scrollTop >= parentTop && scrollTop <= parentTop+parentH) ||
				(scrollB >= parentTop && scrollB <= parentTop+parentH) ||
				(scrollTop < parentTop && scrollB > parentTop+parentH)
		) {
			marginT = -1 * (scrollTop - parentTop) * 0.3 * elK;
			el.css('margin-top', marginT);
		}
	});
});
```