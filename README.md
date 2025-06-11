# Анимированный UI

## Срок сдачи работ

Последний коммит и пул реквест должен быть оформлен до ???

## Цель:

Научиться использовать Javascript для анимации DOM элементов

Вам дана верстка, стили не в полном объеме и отстутсвует JS. Отсутствуют такие элементы как:

_Ознакомиться с тем как они должны выглядеть можно [здесь](https://21isr.github.io/uidev-lab17/)_

Ваша задача дописать сайт

## Теория

### Intersection Observer

__Intersection Observer API__ — это веб-API в JavaScript, который позволяет асинхронно отслеживать пересечения целевого элемента с его родительским элементом или с областью видимости документа (viewport).

_Простыми словами:_ Intersection Observer "наблюдает" за элементами на странице и сообщает, когда они входят в зону видимости пользователя или выходят из неё.

```Javascript
const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
        // Будет исполняться код ниже, если элемент попадает в viewport
        if (entry.isIntersecting) {
            // Ниже проводится проверка на тип элемента
            if (entry.target.classList.contains("stagger-item")) {
                const staggerItems =
                    entry.target.parentElement.querySelectorAll(".stagger-item")
                staggerItems.forEach((item, index) => {
                    setTimeout(() => {
                        item.classList.add("animate")
                    }, index * 200)
                })
            } else {
                entry.target.classList.add("animate")
            }

            // Отключаем обсервер на элементы, после того, как его анимировали
            observer.unobserve(entry.target)
        }
    })
})
```

### Определение анимированных элементов

Это делается через классы, где описан тип анимации:

```Javascript
const observer = new IntersectionObserver((entries) => {
    entries.forEach((entry) => {
        // Логика при попадании во вьюпорт, то есть доавляем класс "animate"
        // ...
    })
})

document
    .querySelectorAll(
        ".slide-left, .slide-right"
    )
    // Вешаем слушателя созданного выше на элементы
    .forEach((el) => {
        observer.observe(el)
    })
```

```css
.slide-left {
    opacity: 0;
    transform: translateX(-100px);
    transition: all 1s ease-out;
}

.slide-left.animate {
    opacity: 1;
    transform: translateX(0);
}
```

_У таких элементов есть два класса `slide-left` это изначальное положение, а класс `animate` это то, где должен быть файл когда элемент попадает во вьюпорт_

### Создание анимированных частиц

Создаются радномные частицы, которые двигаются

```Javascript
function createParticles() {
    const particlesContainer = document.querySelector(".particles")
    const particleCount = 50

    for (let i = 0; i < particleCount; i++) {
        // Создаем элемент частицы
        const particle = document.createElement("div")
        particle.classList.add("particle")
        particle.style.left = Math.random() * 100 + "%"
        // Рандомизируем его, чтобы не выглядил статично
        particle.style.animationDelay = Math.random() * 15 + "s"
        particle.style.animationDuration = Math.random() * 10 + 10 + "s"
        particlesContainer.appendChild(particle)
    }
}
```

## Как сдавать

1. Создайте форк репозитория в организации `21ISR` с названием `uidev-lab16-вашафамилия`
2. Используя ветку `wip` сделайте задание
3. Зафиксируйте изменения в вашем репозитории
4. Когда документ будет готов - создайте пул реквест из ветки `wip` (вашей) на ветку `main` (тоже вашу) и укажите меня ([ktkv419](https://github.com/ktkv419)) как reviewer

**Не мержите сами коммит**, это сделаю я после проверки задания
