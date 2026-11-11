# Water Gallon

A water-intake tracker built around one visual idea: a large gallon that fills
as you mark cups off below it.

**[Live demo →](https://vette1123.github.io/Water-Gallon/)**

## How it behaves

The cups work as a single level. Clicking any cup
fills every cup before it, and the gallon rises to match. Clicking a filled cup
drains everything above it.

The click handler has one special case:

```js
function highlightCups(index) {
  if (
    smallCups[index].classList.contains('full') &&
    !smallCups[index].nextElementSibling.classList.contains('full')
  ) {
    index--
  }
  smallCups.forEach((cup, index2) => {
    if (index2 <= index) cup.classList.add('full')
    else cup.classList.remove('full')
  })
  updateBigCup()
}
```

If you click the last filled cup (the current level), the handler steps the
index back by one so the cup empties. Without that, tapping the level you are
already at does nothing.

## Derived, not stored

The percentage, the remaining litres and the height of the fill are never held
in a variable. They are recomputed from a single count of how many cups
currently carry the `full` class, every time anything changes:

```js
const fullCups = document.querySelectorAll('.cup-small.full').length
percentage.style.height = `${(fullCups / totalCups) * 330}px`
percentage.innerText = `${(fullCups / totalCups) * 100}%`
```

Storing them would mean four values to keep in agreement, and a label reading
75% while the liquid sits at 50% would follow quickly. With one count
recomputed each time, they cannot drift apart.

## Stack

Plain HTML, CSS and JavaScript. No dependencies, no build step. The fill is a
single element whose height is set in pixels against the cup's 330px body, so
the animation is one CSS transition.

## Known limitations

- Nothing persists. A refresh resets the day. `localStorage` keyed by date
  would be a handful of lines.
- The goal is hardcoded in the markup.
- The percentage is not rounded: seven of eight cups renders as
  `87.5%`.

## Running locally

```bash
git clone https://github.com/Vette1123/Water-Gallon.git
cd Water-Gallon
# open index.html — no build, no server needed
```

---

Built by [Mohamed Gado](https://mohamedgado.com)
