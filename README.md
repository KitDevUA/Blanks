# Мои заготовки и наработки
<a href="https://gist.github.com/fvcproductions/1bfc2d4aecb01a834b46" target="_blank">**(Оформление README.md)**</a>

## Базовое
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


## Удобные @media-запросы (для мобильной адаптации)
```SASS
=r($width)
	@media only screen and (max-width: $width+ "px")
		@content
=rmin($width)
	@media only screen and (min-width: $width+ "px")
		@content

=rh($height)
	@media only screen and (max-height: $height+ "px")
		@content
=rhmin($height)
	@media only screen and (min-height: $height+ "px")
		@content

=rfromto($widthFrom, $widthTo)
	@media only screen and (min-width: $widthFrom+ "px") and (max-width: $widthTo+ "px")
		@content
```


## Mixin для изменения скролбара (дорабатывается)
```SASS
=scrollbars($size, $foreground-color, $background-color)
	::-webkit-scrollbar
		width:  $size
		height: $size
	::-webkit-scrollbar-thumb
		background: $foreground-color
	::-webkit-scrollbar-track
		background: $background-color
	body
		scrollbar-face-color: $foreground-color
		scrollbar-track-color: $background-color
```


## Mixin для placeholder
```SASS
=placeholder()
	&::-webkit-input-placeholder
		@content
	&:-moz-placeholder
		@content
	&::-moz-placeholder
		@content
	&:-ms-input-placeholder
		@content
```


## Mixin для keyframes
```SASS
=keyframes($name)
	@-webkit-keyframes #{$name}
		@content
	@-moz-keyframes #{$name}
		@content
	@-ms-keyframes #{$name}
		@content
	@keyframes #{$name}
		@content

+keyframes(example)
	0%
		background-color: red
	100%
		background-color: blue
animation: example 2s infinite linear
```


## Mixin для преобразования пикселей в vw
```SASS
@function vwu($px, $viewport)
	@return $px / $viewport * 100vw
```



## Mixin для преобразования пикселей в %
```SASS
@function percent($px, $width)
	@return $px / $width * 100%
```



## Автозамена для smart-mobile.sass
### Stage 1
```
\: ([0-9.-]+)px
: vw($1)
```
### Stage 2
```
\: ([0-9.-]+)px
: vw($1)
```



## Плавный скролинг к элементу
```javascript
// Scroll to
$('.scrollTo').click( function(){
	var scroll_el	= $(this).attr('href'),
		speed		= $(this).is("[data-speed]") ? +$(this).attr('data-speed') : 500;
	if ($(scroll_el).length != 0)
		$('html, body').animate({ scrollTop: $(scroll_el).offset().top }, speed);
	return false;
});
// /Scroll to
```

## .htaccess редирект на https
```
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteCond %{HTTP:X-Forwarded-Proto} !https
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
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
		display: none
		position: absolute
		transition: 0.1s
		transition-timing-function: linear
		z-index: 1
		&.el1
			width: 300px
			top: calc(50% + 0px)
			left: calc(50% + 0px)
```
```javascript
// floating elements
let screenH		= $(window).height(),
	scrollTop	= $(window).scrollTop(),
	scrollB		= scrollTop + screenH;

$(window).scroll(function() {
	scrollTop	= $(window).scrollTop();
	scrollB		= scrollTop + screenH;
	floating();
});

function floating() {
	$('.floating').each(function() {
		var el			= $(this),
			elK			= el.attr('data-k') ? +el.attr('data-k') : 1,
			direction	= el.attr('data-direction') ? -1 : 1,
			parent		= el.parent(),
			parentTop	= parent.offset().top,
			parentH		= parent.outerHeight(),
			marginT		= 0,
			rotate		= 0;

		if (
				(scrollTop >= parentTop && scrollTop <= parentTop+parentH) ||
				(scrollB >= parentTop && scrollB <= parentTop+parentH) ||
				(scrollTop < parentTop && scrollB > parentTop+parentH)
		) {
			marginT	= -1 * (scrollTop - parentTop) * 0.3 * elK;
			rotate	= direction * (scrollTop - parentTop) * 0.05 * elK;
			el.css({
				display:			'block',
				'margin-top':		marginT,
				WebkitTransform:	'rotate(' + rotate + 'deg)',
				'-moz-transform':	'rotate(' + rotate + 'deg)',
			});
		}
	});
}
floating();
// /floating elements
```