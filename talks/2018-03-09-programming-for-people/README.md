---
author: Tim Shedor
date: 2018.03.09
title: Programming for People
subtitle: Write code for someone you'll never meet
---

[Watch instead](https://www.youtube.com/watch?v=oX5VYDxvYi8)

# Why?

---

## Every time our tool brings an incomprehensible error message, someone out there decides they're just not cut out for programming.
*Dan Abramov, “[The Melting Pot of JavaScript](https://www.youtube.com/watch?v=G39lKaONAlA)”*

---

#### When a function is invoked with this pattern, this is bound to the global object. This was a mistake in the design of the language. Had the language been designed correctly, when the inner function is invoked, this would still be bound to the this variable of the outer function. A consequence of this error is that a method cannot employ an inner function to help it do its work because the inner function does not share the method’s access to the object as its this is bound to the wrong value. Fortunately, there is an easy workaround. If the method defines a variable and assigns it the value of this, the inner function will have access to this through that variable.
*Douglas Crockford, “JavaScript: The Good Parts”*

---

```javascript
function() {
  var that = this;

  var helper = function() {
    that.value = add(that.value, that.value);
  };

  helper();
}
```

*Douglas Crockford, “JavaScript: The Good Parts”*

---

# Doug eats bananas.

# Doug likes bananas.

---

```javascript
function () {
  // Doug eats bananas.

  // He likes bananas.
}
```

---

# Doug eats bananas.

---

# Doug likes bananas.

---


```javascript
function () {
  // Doug eats bananas.

}

// He likes bananas.
// Who is he???
```

$-- ---

$-- ![Who is he](https://media.giphy.com/media/39xzJMrW1o3OFjLqdF/giphy.gif "Who is he")

$-- ---

```javascript
  // He is Doug.

  var helper = function() {
    // He likes bananas.
  }

  helper();
  // => Doug likes bananas.
}
```

---

```javascript
function() {
  var that = this;

  var helper = function() {
    that.value = add(that.value, that.value);
  };

  helper();
}
```

*Douglas Crockford, “JavaScript: The Good Parts”*

---

```javascript
function() {
  var that = 'Doug';

  var helper = function() {
    return `${that} likes bananas.`;
  };

  helper();
  // => Doug likes bananas.
}
```

---

```javascript
function bananaEdict() {
  return `${this} likes bananas.`;
}

bananaEdict.call('Doug');
// => Doug likes bananas.

bananaEdict();
// => window likes bananas.
// ....or God likes bananas.

// _THEY_ each like bananas.
$('aside').each(function() {
  var currentAside = $(this);
  console.log(`${currentAside} likes bananas.`);
});
```

---

## Every time our tool brings an incomprehensible error message, someone out there decides they're just not cut out for programming.
*Dan Abramov, “[The Melting Pot of JavaScript](https://www.youtube.com/watch?v=G39lKaONAlA)”*

---

```javascript
$('#js-soup-form select').change(function() {
  if ($(this).val()) {
    $('.soup').fadeOut();
    $(`.soup[data-vegetable="${$(this).val()}"]`).fadeIn();
  } else {
    $('.soup').fadeIn();
  }
});
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();

  if (selectedVegetable) {
    $('.soup').fadeOut();
    $(`.soup[data-vegetable="${selectedVegetable}"]`).fadeIn();
  } else {
    $('.soup').fadeIn();
  }
});
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();
  const vegetableSelector = `.soup[data-vegetable="${selectedVegetable}"]`;
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();
  const vegetableSelector = `.soup[data-vegetable="${selectedVegetable}"]`;

  if (selectedVegetable) {
    $('.soup').fadeOut();
    $(vegetableSelector).fadeIn();
  } else {
    $('.soup').fadeIn();
  }
});
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();
  const vegetableSelector = `.soup[data-vegetable="${selectedVegetable}"]`;
  const soups = $('.soup');
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();
  const vegetableSelector = `.soup[data-vegetable="${selectedVegetable}"]`;
  const $soups = $('.soup');
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();
  const vegetableSelector = `.soup[data-vegetable="${selectedVegetable}"]`;
  const

  if (selectedVegetable) {
    $soups.fadeOut();
    $(vegetableSelector).fadeIn();
  } else {
    $soups.fadeIn();
  }
});
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();
  const $soupsBySelectedVegetable = $('.soup[data-vegetable="${selectedVegetable}"]');
  const $soups = $('.soup');

  if (selectedVegetable) {
    $soups.fadeOut();
    $soupsBySelectedVegetable.fadeIn();
  } else {
    $soups.fadeIn();
  }
});
```

---

```javascript
$('#js-soup-form select').change(function() {
  const selectedVegetable = $(this).val();
  const selectedVegetableAttribute = `[data-vegetable="${selectedVegetable}"]`;
  const $soups = $('.soup');
  const $soupsBySelectedVegetable = $soups.has(selectedVegetableAttribute);

  if (selectedVegetable) {
    $soups.fadeOut();
    $soupsBySelectedVegetable.fadeIn();
  } else {
    $soups.fadeIn();
  }
});
```

---

# What makes a good programmer?

---
