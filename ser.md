---
title: "SER"
date: 2024-12-04
---

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Actividad: Ser</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #FFCB56;
            margin: 0;
            padding: 20px;
        }
        h2 {
            text-align: center;
        }
        .container {
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
        }
        .drag-box, .drop-zone {
            display: inline-block;
            padding: 5px 10px;
            margin: 5px;
            background-color: #fff;
            border: 1px solid #ccc;
            border-radius: 5px;
            text-align: center;
            font-size: 14px;
        }
        .drop-zone {
            min-width: 60px;
            height: auto;
            border: 2px dashed #ccc;
            vertical-align: baseline;
        }
        .button-container {
            text-align: center;
            margin-top: 20px;
        }
        .hidden {
            display: none;
        }
        .correct {
            border-color: green;
            color: green;
        }
        .incorrect {
            border-color: red;
            color: red;
        }
    </style>
</head>
<body>
    <h2>Actividad: Las formas del verbo <em>ser</em></h2>
    <div class="container">
        <p>Arrastra las palabras correctas al espacio correspondiente:</p>
        <div id="sentence">
            <p id="line1">Yo <div class="drop-zone" data-answer="soy"></div> el mayor de tres (3) hermanos. ¿Cuántos hermanos hay en total? <div class="drop-zone" data-answer="tres"></div>.</p>
            <p id="line2">Tú <div class="drop-zone" data-answer="eres"></div> el dueño de cinco (5) manzanas. Si vendes dos (2), ¿cuántas te quedan? <div class="drop-zone" data-answer="tres"></div>.</p>
            <p id="line3">Él <div class="drop-zone" data-answer="es"></div> el encargado de contar los libros. Hay diez (10) libros en cada estante y cuatro (4) estantes. ¿Cuántos libros hay? <div class="drop-zone" data-answer="cuarenta"></div>.</p>
            <p id="line4">Nosotros <div class="drop-zone" data-answer="somos"></div> un equipo de seis (6) personas. Si necesitamos dividirnos en dos grupos iguales, ¿cuántas personas hay en cada grupo? <div class="drop-zone" data-answer="tres"></div>.</p>
            <p id="line5">Ellos <div class="drop-zone" data-answer="son"></div> amigos que tienen entre todos quince (15) pelotas. Si cada uno tiene tres (3) pelotas, ¿cuántos amigos hay? <div class="drop-zone" data-answer="cinco"></div>.</p>
        </div>
        <div id="options">
            <!-- Conjugations and numbers will be dynamically scrambled -->
        </div>
        <div class="button-container">
            <button id="checkAnswers">Check Answers</button>
            <button id="retry" class="hidden">Retry</button>
            <button id="showTranslations" class="hidden">See English Translations</button>
        </div>
        <p id="score" style="text-align: center; font-weight: bold;"></p>
    </div>

    <script>
        // Conjugations and number words
        const conjugations = ["soy", "eres", "es", "somos", "son"];
        const numbers = ["tres", "cinco", "cuarenta", "tres", "cinco"];

        // Shuffle the items
        function shuffle(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }
        shuffle(conjugations);
        shuffle(numbers);

        // Insert scrambled items into the options container
        const optionsContainer = document.getElementById('options');
        conjugations.concat(numbers).forEach(item => {
            const dragBox = document.createElement('div');
            dragBox.className = 'drag-box';
            dragBox.draggable = true;
            dragBox.textContent = item;
            dragBox.dataset.value = item;
            optionsContainer.appendChild(dragBox);
        });

        // Translations for sentences
        const translations = {
            line1: "I am the oldest of three (3) siblings. How many siblings are there in total? Three.",
            line2: "You are the owner of five (5) apples. If you sell two (2), how many do you have left? Three.",
            line3: "He is in charge of counting the books. There are ten (10) books on each shelf and four (4) shelves. How many books are there? Forty.",
            line4: "We are a team of six (6) people. If we need to divide into two equal groups, how many people are in each group? Three.",
            line5: "They are friends who have fifteen (15) balls altogether. If each has three (3) balls, how many friends are there? Five."
        };

        // Drag-and-Drop Functionality
        const dragBoxes = document.querySelectorAll('.drag-box');
        const dropZones = document.querySelectorAll('.drop-zone');
        const checkAnswersButton = document.getElementById('checkAnswers');
        const retryButton = document.getElementById('retry');
        const showTranslationsButton = document.getElementById('showTranslations');
        const scoreDisplay = document.getElementById('score');

        dragBoxes.forEach(box => {
            box.addEventListener('dragstart', e => {
                e.dataTransfer.setData('text/plain', e.target.dataset.value);
            });
        });

        dropZones.forEach(zone => {
            zone.addEventListener('dragover', e => {
                e.preventDefault();
            });

            zone.addEventListener('drop', e => {
                e.preventDefault();
                const droppedValue = e.dataTransfer.getData('text/plain');
                if (!zone.textContent) {
                    zone.textContent = droppedValue;
                }
            });
        });

        // Check Answers Functionality
        checkAnswersButton.addEventListener('click', () => {
            let correctCount = 0;
            dropZones.forEach(zone => {
                if (zone.textContent === zone.dataset.answer) {
                    zone.classList.add('correct');
                    correctCount++;
                } else {
                    zone.classList.add('incorrect');
                }
            });
            scoreDisplay.textContent = `Your score: ${correctCount} / ${dropZones.length}`;
            retryButton.classList.remove('hidden');
            if (correctCount === dropZones.length) {
                showTranslationsButton.classList.remove('hidden');
            }
            checkAnswersButton.disabled = true; // Disable the button after checking
        });

        // Retry Functionality
        retryButton.addEventListener('click', () => {
            dropZones.forEach(zone => {
                zone.textContent = '';
                zone.classList.remove('correct', 'incorrect');
            });
            scoreDisplay.textContent = '';
            checkAnswersButton.disabled = false;
            retryButton.classList.add('hidden');
            showTranslationsButton.classList.add('hidden');
        });

        // Show Translations Functionality
        showTranslationsButton.addEventListener('click', () => {
            Object.keys(translations).forEach(lineId => {
                const line = document.getElementById(lineId);
                line.innerHTML += ` <em>(${translations[lineId]})</em>`;
            });
            showTranslationsButton.disabled = true; // Disable after showing translations
        });
    </script>
</body>
</html>
