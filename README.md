# Мои заготовки и наработки
[**(Оформление README.md #1)**](https://gist.github.com/Jekins/2bf2d0638163f1294637)<br>
[**(Оформление README.md #2)**](https://github.com/sandino/Markdown-Cheatsheet/blob/master/README.md)<br>


## Basic
**jQuery**
```HTML
<script type="text/javascript" src="https://ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"></script>
```

**Мобильная версия**
```HTML
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
```

**Vue js - простое подключение**
[vue.js](https://foxk.ru/libs/vue.js)
```HTML
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```


## @media - Удобные @media-запросы (для мобильной адаптации)
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


## Автозамена для _smart-mobile.sass
### Медиа-параметры
```
@media only screen and (min-width: 992PX) and (max-width: 1450PX)
```
Stage 1
```
([+-]*)([ ]*)([0-9.-]+)px\)
$1$2#{vw($3)})
```
Stage 2
```
 ([0-9.-]+)px
 vw($1)
```


## .htaccess редирект на https
```
RewriteEngine On
RewriteCond %{HTTPS} off
RewriteCond %{https:X-Forwarded-Proto} !https
RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```


## ScrollTo - Плавный скролинг к элементу
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


## Timer - Таймер, учитывающий часовые зоны
```php
<?php
	$addHours	= 0; // Сколько добавить часов
	$addMinutes	= 15; // Сколько добавить минут
	
	
	if ( isset($_COOKIE['timestamp']) && (int)$_COOKIE['timestamp'] > 0 )
		$timestamp	= (int)$_COOKIE['timestamp'];
	else {
		$timestamp	= strtotime( gmdate("Y-m-d H:i:s", time()) ); // gmt
		$timestamp	= $timestamp + $addHours * 60 * 60 + $addMinutes * 60; // add time
		setcookie('timestamp', $timestamp, strtotime('+1 days'), '/');
	}
	
?>
```

```html
<script>
	var	eDay	= '<?= date( 'd', $timestamp ) ?>',
		eMonth	= '<?= date( 'm', $timestamp ) ?>',
		eYear	= '<?= date( 'Y', $timestamp ) ?>',
		eHour	= '<?= date( 'H', $timestamp ) ?>',
		eMinute	= '<?= date( 'i', $timestamp ) ?>',
		eSecond	= '<?= date( 's', $timestamp ) ?>';
</script>

<div class="timer">
	<span id="tHours" class="number">00</span>
	<span class="delimeter">:</span>
	<span id="tMinutes" class="number">00</span>
	<span class="delimeter">:</span>
	<span id="tSeconds" class="number">00</span>
</div>
```

```javascript
// Timer
function prefInt( number ) {
	let result = number;
	if ( +number < 10 )
		result	= "0"+number;
	return result;
}

function timerTic() {
	let	dateNow		= new Date(),
		dateFuture	= new Date(eYear+'-'+eMonth+'-'+eDay+'T'+eHour+':'+eMinute+':'+eSecond+'.000Z'),
		delta		= 0,
		days		= 0,
		hours		= 0,
		minutes		= 0,
		seconds		= 0;
	
	if ( dateFuture > dateNow ) {
		delta	= Math.abs(dateFuture - dateNow) / 1000;
		
		days	= Math.floor(delta / 86400);
		delta	-= days * 86400;
		
		if ( days > 0 ) {
			hours	= Math.floor(delta / 3600) % 24;
			delta	-= hours * 3600;
			hours	= hours + (days * 24);
		}
		else {
			hours	= Math.floor(delta / 3600) % 24;
			delta -= hours * 3600;
		}
		
		minutes	= Math.floor(delta / 60) % 60;
		delta -= minutes * 60;
		
		seconds	= Math.floor(delta % 60);  // in theory the modulus is not required
	}
	
	$('#tHours').text( prefInt(hours) );
	$('#tMinutes').text( prefInt(minutes) );
	$('#tSeconds').text( prefInt(seconds) );
}
timerTic();
setInterval( timerTic, 1000 );
// /Timer
```


## Slick-slider
[slick.js](https://foxk.ru/slider/slick.js)<br>
[slick.css](https://foxk.ru/slider/slick.css)<br>
[slick-theme.css](https://foxk.ru/slider/slick-theme.css) (необязательно)
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


## Popup - Всплывающее окно
```HTML
<div id="popupID" class="popup">
	<i class="bg"></i>
	<div class="box">
		<button class="close"></button>
		<div class="inner">
			
		</div>
	</div>
</div>
```
```SASS
.popup
	width: 100%
	height: 100%
	justify-content: center
	align-items: center
	position: fixed
	top: 0
	left: 0
	right: 0
	bottom: 0
	z-index: 10
	&:not(.opened)
		opacity: 0
		z-index: -10
	i.bg
		width: 100%
		height: 100%
		background-color: rgba(#000, .7)
		position: absolute
		top: 0
		left: 0
		right: 0
		bottom: 0
		z-index: 1
	.box
		width: calc(100% - 40px)
		max-height: 90%
		max-width: 1000px
		padding-top: 40px
		background-color: #fff
		position: relative
		z-index: 2
		overflow: hidden
		flex-shrink: 0
		button.close
			width: 35px
			height: 35px
			border-radius: 50%
			border: 2px solid #000
			background-color: #fff
			display: flex
			justify-content: center
			align-items: center
			position: absolute
			top: 10px
			right: 10px
			z-index: 5
			&::before,
			&::after
				content: ''
				display: block
				width: 55%
				height: 2px
				background-color: #000
				position: absolute
			&::before
				transform: rotate(-45deg)
			&::after
				transform: rotate(45deg)
			&:hover
				&::before,
				&::after
					width: 70%
		.inner
			padding: 0 15px 30px
			overflow-y: auto
			*
				flex-shrink: 0
```
```javascript
// Popups
function openPopup( $id ) {
	$('body').addClass('popupOpened');
	$($id).addClass('opened');
}
$('body').on('click', '.openPopup', function() {
	openPopup( $(this).attr('href') );
	return false;
});

function popupClose( $id ) {
	$('body').removeClass('popupOpened');
	$('.popup.opened').removeClass('opened');
}
$('.popup i.bg, .popup button.close').click( function() {
	popupClose( '#'+$(this).closest('.popup').attr('id') );
} );
// /Popups
```


## Paralax - Параллакс элементов при склолинге
```HTML
<div class="parent">
	<i class="parallax el1" data-k="0.8"></i>
</div>
```
```SASS
.parent
	position: relative
.parallax
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


## wow.js + animate.css
[animate.css](https://foxk.ru/animations/animate.css)<br>
[wow.min.js](https://foxk.ru/animations/wow.min.js)<br>
[Документация по animate.css](https://animate.style/)
```javascript
var	wow = new WOW({
	boxClass:		'wow',
	animateClass:	'animated',
	offset:			0,
	mobile:			false,
	live:			true,
});
wow.init();
```
```HTML
<div class="wow fadeIn"></div>
```
```
data-wow-duration="" Длительность анимации
data-wow-delay="" Задержка
data-wow-offset="" Отступ сверху
data-wow-iteration="" Количество повторов
```


## IMask - Маска для ввода номера телефона для нескольких стран
[imask.js](https://foxk.ru/imask/imask.js)
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


















