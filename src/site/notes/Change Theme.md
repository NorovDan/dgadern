---
{"dg-publish":true,"permalink":"/change-theme/"}
---

**ФИО:** 2022-ФГиИБ-ИБ-1б Воронов Даниил Алексеевич 

```html
<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Смена темы</title>
    <style>
        body {
            background-color: white;
            color: black;
            transition: background-color 0.3s, color 0.3s;
        }

        .dark-theme {
            background-color: black;
            color: white;
        }

        .container {
            text-align: center;
            padding: 50px;
        }

        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Смена темы</h1>
        <button id="theme-toggle">Сменить тему</button>
    </div>

    <script>
        document.addEventListener("DOMContentLoaded", function() {
            const themeToggleButton = document.getElementById("theme-toggle");
            let isDarkTheme = false;

            themeToggleButton.addEventListener("click", function() {
                isDarkTheme = !isDarkTheme;

                if (isDarkTheme) {
                    document.body.classList.add("dark-theme");
                    themeToggleButton.textContent = "Светлая тема";
                } else {
                    document.body.classList.remove("dark-theme");
                    themeToggleButton.textContent = "Темная тема";
                }
            });
        });
    </script>
</body>
</html>
```

