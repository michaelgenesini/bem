# BEM
Just a personal TL;DR on BEM

## Table of contents

1. [BEM anatomy](#bem-anatomy)
1. [Entity](#entity)
1. [Block](#block)
1. [Element](#element)
1. [Modifier](#modifier)

## BEM anatomy

```css
/*
 * Entity
 * A set of Elements and Modifiers related to a single Block
 */

/*
 * Block
 * The single root-class of your Entity
 */

.todoItem { }

/*
 * Element
 * Any number of Block-descendants
 */

.todoItem__mark { }
.todoItem__text { }

/*
* Modifier
* Any number of Block/Element modifiers
*/

.todoItem--complete { }
.todoItem--due { }
```

I prefer to have a single camel-cased word for each block so I can easy visually distinguish Blocks Elements and Modifiers.
For Modifiers I prefer the most popular [`--` dialect](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/) by Harry Roberts. Using `--` for modifiers removes a small ambiguity around [key/value modifiers](https://en.bem.info/methodology/naming-convention/#element-modifier).

**[⬆ back to top](#table-of-contents)**

## Entity
An Entity is a collections of Elements and Modifiers, related to a single Block. Entities are analogous to "modules" or "components" in other CSS philosophies.

Practically, Entities should live in a single file and be easily transportable to other projects.

**[⬆ back to top](#table-of-contents)**

## Block
A Block is the single, root-level, classes of any Entity.

```css
.todoItem { }
```

A bunch of Blocks:

```css
.report { }
.reportItem { }
.reportSubtotal { }

.person { }
.personName { }
.personChildList { }

.adminPerson { }
.adminPersonResponsibilitiesList { }
.adminPersonResponsibilitiesItem { }
```

### Relationships
* Block `belongs_to` Entity
* Block `has_many` Elements
* Block `has_many` Modifiers

**[⬆ back to top](#table-of-contents)**

## Element
An Element is anything nested in a Block.

*"Descendent" is actually a better word but `BDM` doesn't roll off the tongue.*

Here are some elements:

```css
.todoItem__mark { }
.todoItem__text { }
.todoItem__input { }
.todoItem__deleteButton { }
```

Elements can have Modifiers. Though they're not preferred.

```css
.todoItem__deleteButton--blingy { }
```

### Prefer Block-Modifiers
I find Block-Modifiers to be more maintainable than Element-Modifiers. In most cases, Element-Modifiers are partial Block-Modifiers waiting to grow up. You can avoid refactoring by preferring Block-Modifiers.

### No grandchildren

BEM supports only one level of element. If you find yourself reaching for Element-Elements (grandchildren), you have two options:
* Flatten out the Elements
* Create a new Block

#### This is incorrect
```
<li class="todoItem">
  <header className="todoItem__header">
    <span className="todoItem__header__text">Mow Lawn</span>
  </header>
</li>
```

#### Option 1: Flatten
```
<li class="todoItem">
  <header className="todoItem__header">
    <span className="todoItem__headerText">Mow Lawn</span>
  </header>
</li>
```

#### Option 2: New Block
```
<li class="todoItem">
  <header className="todoItemHeader">
    <span className="todoItemHeader__text">Mow Lawn</span>
  </header>
</li>
```

Now `.todoItemHeader` can be composed with Elements from the `.todoItem` Entities.

```
<li class="todoItem">
  <header className="todoItem__header todoItemHeader">
    <span className="todoItem-header__text">Mow Lawn</span>
  </header>
</li>
```

#### Why?

This may seem pedantic but it's critical to how BEM scales. Grandchildren introduce relationships into Entities. Once you have relationships, how many descendants do you cut off at? BEM makes it easy and cuts it off at 1.

### Relationships
* Entity `belongs_to` Block
* Entity `has_many` Modifiers

**[⬆ back to top](#table-of-contents)**

## Modifier
Modifiers are like HTML attributes. They alter Blocks and Elements. They can be **boolean** or **key/value** pairs.

```css
/* boolean */
.todoItem--hidden { }

/* named attributes*/
.todoItem--sizeSmall { }
.todoItem--typeRadio { }

/* Element_Modifier */
.todoItem__header--fancy { }
```

### Prefer Block-Modifiers
I find Block-Modifiers to be more maintainable than Element-Modifiers. In most cases, Element-Modifiers are partial Block-Modifiers waiting to grow up. You can avoid refactoring by preferring Block-Modifiers.

This code commonly gets outgrown:

```html
.todoItem__header--blingy { }
```

Prefer this by default:

```html
.todoItem--blingy { }
.todoItem--blingy .todoItem__header { }
```

### Alternatives
The BEM approach to Modifiers is hotly contested. This stems from a desire to type less or a misunderstanding of early BEM documentation. Granted, the early docs were in pretty poor English.

While I like variants with global states, e.g., `.is-visible`, it breaks the BEM model. More relevant, it makes utility-class libraries (e.g., minions.css and tachyons) harder to use.

### Relationships
* Modifier `belongs_to` Block
* Modifier `belongs_to` Element

**[⬆ back to top](#table-of-contents)**

## Credits
Re-adapted methodology from [pratical-bem](https://github.com/chantastic/practical-bem) by [Michael Chan](https://github.com/chantastic)
