<!DOCTYPE html>
<html lang="pt">
<head>
    <meta charset="UTF-8">
    <title>Slot Simples com Bônus</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f0f0f0;
        }
        .slot-machine {
            display: inline-block;
            margin-top: 50px;
        }
        .reel {
            display: inline-block;
            width: 100px;
            height: 100px;
            border: 2px solid #333;
            margin: 0 5px;
            line-height: 100px;
            font-size: 50px;
            background-color: white;
            transition: transform 0.5s ease-in-out;
        }
        #spin-button {
            margin-top: 20px;
            padding: 10px 20px;
            font-size: 16px;
        }
    </style>
</head>
<body>

    <div class="slot-machine">
        <div class="reel" id="reel1">🐯</div>
        <div class="reel" id="reel2">🥇</div>
        <div class="reel" id="reel3">🂡</div>
    </div>

    <button id="spin-button">Girar</button>

    <script>
        const symbols = ['🐯', '🥇', '🂡', '🍀', '🎁'];
        let credit = 10; // crédito inicial

        const spinButton = document.getElementById('spin-button');
        const reels = [
            document.getElementById('reel1'),
            document.getElementById('reel2'),
            document.getElementById('reel3')
        ];

        spinButton.addEventListener('click', () => {
            spinButton.disabled = true; // Desabilita o botão enquanto gira

            // Adiciona animação de rotação
            reels.forEach((reel, index) => {
                setTimeout(() => {
                    reel.style.transform = `rotateY(${360 * (index + 1)}deg)`;
                }, index * 100);
            });

            // Girar os rolos aleatoriamente após a animação
            setTimeout(() => {
                reels.forEach(reel => {
                    const randomSymbol = symbols[Math.floor(Math.random() * symbols.length)];
                    reel.textContent = randomSymbol;
                    reel.style.transform = 'rotateY(0deg)'; // Resetar a rotação
                });

                // Verificar resultado e bônus
                checkResults();

                // Reabilitar o botão após a animação
                spinButton.disabled = false;
            }, 600); // Tempo total da animação
        });

        function checkResults() {
            const reelValues = reels.map(reel => reel.textContent);
            let win = false;
            let bonus = false;

            // Exemplo de lógica: se todos os rolos forem iguais, ganha
            if (reelValues.every(symbol => symbol === reelValues[0])) {
                win = true;
                credit += 5; // Ganho padrão
            } else {
                credit -= 1; // Perda padrão
            }

            // Chance aleatória de bônus
            if (Math.random() < 0.1) { // 10% de chance
                bonus = true;
                credit += 10; // Bônus extra
                alert('Bônus liberado! +10 créditos!');
            }

            if (win) {
                alert('Você ganhou! +5 créditos!');
            } else {
                alert('Você perdeu! -1 crédito.');
            }

            // Mostrar o crédito atual no console
            console.log('Crédito atual:', credit);
        }
    </script>

</body>
</html>
``
