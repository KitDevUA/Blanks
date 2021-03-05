# Мои заготовки и наработки
[**(Оформление README.md)**](https://gist.github.com/Jekins/2bf2d0638163f1294637)<br>

<br>
## Содержание:
[1. Базовое](#content1)<br>
[2. Удобные @media-запросы](#content2)<br>
[3. Mixin для изменения скролбара](#content3)<br>
[4. Mixin для placeholder](#content4)<br>
[5. Mixin для keyframes](#content5)<br>
[6. Mixin для преобразования пикселей в vw](#content6)<br>
[7. Mixin для преобразования пикселей в %](#content7)<br>
[8. Автозамена для smart-mobile.sass](#content8)<br>
[9. Плавный скролинг к элементу](#content9)<br>
[10. Маска для ввода номера телефона для нескольких стран](#content10)<br>
[11. .htaccess редирект на https](#content11)<br>
[12. Паралакс элементов при склолинге](#content12)<br>
[13. Slick-slider](#content13)<br>



<br>
<a name="content1"></a><br><br>
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


<a name="content2"></a><br><br>
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


<a name="content3"></a><br><br>
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


<a name="content4"></a><br><br>
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


<a name="content5"></a><br><br>
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


<a name="content6"></a><br><br>
## Mixin для преобразования пикселей в vw
```SASS
@function vwu($px, $viewport)
	@return $px / $viewport * 100vw
```


<a name="content7"></a><br><br>
## Mixin для преобразования пикселей в %
```SASS
@function percent($px, $width)
	@return $px / $width * 100%
```


<a name="content8"></a><br><br>
## Автозамена для smart-mobile.sass
### Медиа-параметры
```
@media only screen and (min-width: 992PX) and (max-width: 1450PX)
```
### Stage 1
```
([+-]*)([ ]*)([0-9.-]+)px\)
$1$2#{vw($3)})
```
### Stage 2
```
 ([0-9.-]+)px
 vw($1)
```


<a name="content9"></a><br><br>
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


<a name="content10"></a><br><br>
## Маска для ввода номера телефона для нескольких стран
[https://unpkg.com/imask](https://unpkg.com/imask)
```javascript
// Phone mask
const phoneMaskInputs	= document.querySelectorAll('.phone-mask');
const masksOptions		= {
	mask: [
		{
			mask: '+380 00 000 00 00',
			startsWith: '38',
			country: 'Ukraine'
		},
		{
			mask: '+0 000 000 00 00',
			startsWith: '7',
			country: 'Russia'
		},
		{
			mask: '+375 00 000 00 00',
			startsWith: '37',
			country: 'Belarus'
		},
		{
			mask: '+0000000000000',
			startsWith: '',
			country: 'unknown'
		},
	],
	dispatch: function (appended, dynamicMasked) {
		var number = (dynamicMasked.value + appended).replace(/\D/g,'');

		return dynamicMasked.compiledMasks.find(function (m) {
			return number.indexOf(m.startsWith) === 0;
		});
	}
};

for ( const item of phoneMaskInputs ) {
	new IMask(item, masksOptions);
}
// /Phone mask
```


<a name="content11"></a><br><br>
## .htaccess редирект на https
```
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteCond %{HTTP:X-Forwarded-Proto} !https
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```


<a name="content12"></a><br><br>
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


<a name="content13"></a><br><br>
## Slick-slider
[slick.js](http://foxk.ru/slider/slick.js)<br>
[slick.css](http://foxk.ru/slider/slick.css)<br>
[slick-theme.css](http://foxk.ru/slider/slick-theme.css) (необязательно)
```javascript
// Slider
let $gallery	= $('#gallery .inner');

$gallery.slick({
	infinite:		true,
	slidesToShow:	3,
	slidesToScroll:	1,
	arrows:			false,
	adaptiveHeight:	true,
	swipeToSlide:	true,
	centerMode:		true,
	responsive: [
		{
			breakpoint: 991,
			settings: {
				slidesToShow:	1,
				swipe:			true,
			}
		},
	],
});
$('#gallery').on('click', '.control.prev', function() {
	$gallery.slick('slickPrev');
	return false;
});
$('#gallery').on('click', '.control.next', function() {
	$gallery.slick('slickNext');
	return false;
});
// /Slider
```
