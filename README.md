 <!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kids Quiz Adventure - Class System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @keyframes bounce {
            0%, 20%, 53%, 80%, 100% { transform: translate3d(0,0,0); }
            40%, 43% { transform: translate3d(0,-30px,0); }
            70% { transform: translate3d(0,-15px,0); }
            90% { transform: translate3d(0,-4px,0); }
        }
        @keyframes sparkle {
            0% { transform: scale(0) rotate(0deg); opacity: 0; }
            50% { transform: scale(1) rotate(180deg); opacity: 1; }
            100% { transform: scale(0) rotate(360deg); opacity: 0; }
        }
        @keyframes levelUp {
            0% { transform: scale(1); }
            50% { transform: scale(1.2); }
            100% { transform: scale(1); }
        }
        .bounce { animation: bounce 1s ease-in-out; }
        .sparkle { animation: sparkle 0.8s ease-in-out; }
        .level-up { animation: levelUp 0.6s ease-in-out; }
        .gradient-bg { background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); }
    </style>
</head>
<script>
    window.addEventListener('mouseover', initLandbot, { once: true });
    window.addEventListener('touchstart', initLandbot, { once: true });
    var myLandbot;
    function initLandbot() {
      if (!myLandbot) {
        var s = document.createElement('script');
        s.type = "module"
        s.async = true;
        s.addEventListener('load', function() {
          myLandbot = new Landbot.Popup({
            configUrl: 'https://storage.googleapis.com/landbot.online/v3/H-3085914-A7U41AEGUN06VKTR/index.json',
          });
        });
        s.src = 'https://cdn.landbot.io/landbot-3/landbot-3.0.0.mjs';
        var x = document.getElementsByTagName('script')[0];
        x.parentNode.insertBefore(s, x);
      }
    }
    </script>
  
<body class="gradient-bg min-h-screen font-sans">
    <div class="container mx-auto px-4 py-8 max-w-4xl">
        <!-- Header -->
        <div class="text-center mb-8">
            <h1 class="text-4xl font-bold text-white mb-2">🌟 Kids Quiz Adventure 🌟</h1>
            <p class="text-white/80 text-lg">Progress through classes and unlock harder challenges!</p>
        </div>

        <!-- Class Progress Bar -->
        <div id="classProgress" class="bg-white/20 rounded-2xl p-4 mb-6 hidden">
            <div class="flex justify-between items-center mb-2">
                <div class="text-white font-bold">
                    Current Class: <span id="currentClass">Beginner</span>
                </div>
                <div class="text-white/80">
                    <span id="classXP">0</span>/<span id="classXPNeeded">100</span> XP
                </div>
            </div>
            <div class="bg-white/30 rounded-full h-3">
                <div id="xpBar" class="bg-yellow-400 h-3 rounded-full transition-all duration-500" style="width: 0%"></div>
            </div>
        </div>

        <!-- Main Game Area -->
        <div id="gameArea" class="bg-white rounded-3xl shadow-2xl p-8 mb-8">
            <!-- Welcome Screen -->
            <div id="welcomeScreen" class="text-center">
                <div class="text-6xl mb-4">🎯</div>
                <h2 class="text-3xl font-bold text-gray-800 mb-4">Ready for an Adventure?</h2>
                <p class="text-gray-600 mb-4">Start as a Beginner and work your way up to Expert!</p>
                
                <!-- Class Selection -->
                <div class="mb-6">
                    <h3 class="text-lg font-bold text-gray-700 mb-3">Choose Your Starting Class:</h3>
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-4 max-w-2xl mx-auto">
                        <div class="class-option bg-green-100 border-2 border-green-300 rounded-xl p-4 cursor-pointer hover:bg-green-200 transition-all" data-class="0">
                            <div class="text-2xl mb-2">🌱</div>
                            <div class="font-bold text-green-800">Beginner</div>
                            <div class="text-sm text-green-600">Easy questions</div>
                        </div>
                        <div class="class-option bg-blue-100 border-2 border-blue-300 rounded-xl p-4 cursor-pointer hover:bg-blue-200 transition-all opacity-50" data-class="1">
                            <div class="text-2xl mb-2">⭐</div>
                            <div class="font-bold text-blue-800">Intermediate</div>
                            <div class="text-sm text-blue-600">Medium questions</div>
                            <div class="text-xs text-gray-500">Unlock at 200 XP</div>
                        </div>
                        <div class="class-option bg-purple-100 border-2 border-purple-300 rounded-xl p-4 cursor-pointer hover:bg-purple-200 transition-all opacity-50" data-class="2">
                            <div class="text-2xl mb-2">👑</div>
                            <div class="font-bold text-purple-800">Expert</div>
                            <div class="text-sm text-purple-600">Hard questions</div>
                            <div class="text-xs text-gray-500">Unlock at 500 XP</div>
                        </div>
                    </div>
                </div>
                
                <input type="text" id="playerName" placeholder="Enter your name" class="px-4 py-2 border-2 border-purple-300 rounded-lg mb-4 text-center text-lg">
                <br>
                <button onclick="startQuiz()" class="bg-purple-500 hover:bg-purple-600 text-white font-bold py-3 px-8 rounded-full text-xl transition-all transform hover:scale-105">
                    Start Quiz! 🚀
                </button>
            </div>

            <!-- Quiz Screen -->
            <div id="quizScreen" class="hidden">
                <div class="flex justify-between items-center mb-6">
                    <div class="text-lg font-semibold text-gray-700">
                        Question <span id="questionNumber">1</span> of 10
                    </div>
                    <div class="flex items-center space-x-4">
                        <div class="text-lg font-semibold text-purple-600">
                            Score: <span id="currentScore">0</span>
                        </div>
                        <div class="text-lg font-semibold text-blue-600">
                            XP: +<span id="questionXP">10</span>
                        </div>
                    </div>
                </div>
                
                <div id="difficultyIndicator" class="text-center mb-4">
                    <span class="px-3 py-1 rounded-full text-sm font-bold bg-green-100 text-green-800">
                        🌱 Beginner Level
                    </span>
                </div>
                
                <div class="bg-gradient-to-r from-blue-100 to-purple-100 rounded-2xl p-6 mb-6">
                    <h3 id="questionText" class="text-xl font-bold text-gray-800 mb-4"></h3>
                    <div id="answersContainer" class="space-y-3"></div>
                </div>

                <div class="text-center">
                    <button id="nextButton" onclick="nextQuestion()" class="bg-green-500 hover:bg-green-600 text-white font-bold py-2 px-6 rounded-full hidden transition-all">
                        Next Question →
                    </button>
                </div>
            </div>

            <!-- Results Screen -->
            <div id="resultsScreen" class="hidden text-center">
                <div class="text-6xl mb-4">🎉</div>
                <h2 class="text-3xl font-bold text-gray-800 mb-4">Quiz Complete!</h2>
                <div class="text-2xl font-bold text-purple-600 mb-2">
                    Final Score: <span id="finalScore">0</span>/10
                </div>
                <div class="text-lg font-bold text-blue-600 mb-6">
                    XP Earned: +<span id="xpEarned">0</span>
                </div>
                
                <!-- Class Progress -->
                <div id="classUpgrade" class="bg-gradient-to-r from-yellow-100 to-orange-100 border-2 border-yellow-400 rounded-2xl p-4 mb-6 hidden">
                    <div class="text-3xl mb-2">🎊</div>
                    <div class="font-bold text-yellow-800 text-xl">CLASS UPGRADE!</div>
                    <div id="upgradeText" class="text-yellow-700"></div>
                </div>
                
                <!-- Achievement Badges -->
                <div class="mb-8">
                    <h3 class="text-xl font-bold text-gray-700 mb-4">🏆 Your Achievements</h3>
                    <div id="badgesContainer" class="flex flex-wrap justify-center gap-4"></div>
                </div>

                <button onclick="showLeaderboard()" class="bg-blue-500 hover:bg-blue-600 text-white font-bold py-3 px-6 rounded-full mr-4 transition-all">
                    View Leaderboard 📊
                </button>
                <button onclick="resetQuiz()" class="bg-purple-500 hover:bg-purple-600 text-white font-bold py-3 px-6 rounded-full transition-all">
                    Play Again 🔄
                </button>
            </div>

            <!-- Leaderboard Screen -->
            <div id="leaderboardScreen" class="hidden">
                <div class="text-center mb-6">
                    <h2 class="text-3xl font-bold text-gray-800 mb-2">🏆 Leaderboard</h2>
                    <p class="text-gray-600">Top Quiz Champions by Class!</p>
                </div>
                
                <div id="leaderboardList" class="space-y-3 mb-6"></div>
                
                <div class="text-center">
                    <button onclick="resetQuiz()" class="bg-purple-500 hover:bg-purple-600 text-white font-bold py-3 px-6 rounded-full transition-all">
                        Play Again 🔄
                    </button>
                </div>
            </div>
        </div>

        <!-- Achievement Notification -->
        <div id="achievementNotification" class="fixed top-4 right-4 bg-yellow-400 text-yellow-900 px-6 py-4 rounded-2xl shadow-lg hidden transform transition-all">
            <div class="flex items-center">
                <span class="text-2xl mr-2">🏆</span>
                <div>
                    <div class="font-bold">Achievement Unlocked!</div>
                    <div id="achievementText" class="text-sm"></div>
                </div>
            </div>
        </div>

        <!-- Level Up Notification -->
        <div id="levelUpNotification" class="fixed top-20 right-4 bg-gradient-to-r from-yellow-400 to-orange-400 text-yellow-900 px-6 py-4 rounded-2xl shadow-lg hidden transform transition-all">
            <div class="flex items-center">
                <span class="text-2xl mr-2">🎊</span>
                <div>
                    <div class="font-bold">Class Upgrade!</div>
                    <div id="levelUpText" class="text-sm"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Class system
        const classes = [
            { name: "Beginner", icon: "🌱", color: "green", xpRequired: 0 },
            { name: "Intermediate", icon: "⭐", color: "blue", xpRequired: 200 },
            { name: "Expert", icon: "👑", color: "purple", xpRequired: 500 }
        ];

        // League system - separate leaderboards for each league
        const leagues = [
            { name: "Bronze", icon: "🥉", color: "amber", storageKey: "bronzeLeaderboard" },
            { name: "Silver", icon: "🥈", color: "gray", storageKey: "silverLeaderboard" },
            { name: "Gold", icon: "🥇", color: "yellow", storageKey: "goldLeaderboard" },
            { name: "Platinum", icon: "💎", color: "cyan", storageKey: "platinumLeaderboard" }
        ];

        // Fake leaderboard data for each league - sorted by XP
        const fakeLeaderboardData = {
            bronze: [
                { name: "Rakesh ", score: 10, class: "Beginner", classIcon: "🌱", badges: 5, xp: 1200, totalXP: 1200, date: new Date().toLocaleDateString() },
                { name: "Dharmesh", score: 9, class: "Beginner", classIcon: "🌱", badges: 4, xp: 1150, totalXP: 1150, date: new Date().toLocaleDateString() },
                { name: "Ayushi", score: 8, class: "Beginner", classIcon: "🌱", badges: 3, xp: 1100, totalXP: 1100, date: new Date().toLocaleDateString() },
                { name: "Palak", score: 7, class: "Beginner", classIcon: "🌱", badges: 3, xp: 1050, totalXP: 1050, date: new Date().toLocaleDateString() },
                { name: "sonia", score: 6, class: "Beginner", classIcon: "🌱", badges: 2, xp: 1000, totalXP: 1000, date: new Date().toLocaleDateString() },
                { name: "shadab", score: 8, class: "Intermediate", classIcon: "⭐", badges: 3, xp: 950, totalXP: 950, date: new Date().toLocaleDateString() },
                { name: "faiz", score: 7, class: "Intermediate", classIcon: "⭐", badges: 3, xp: 900, totalXP: 900, date: new Date().toLocaleDateString() },
                { name: "maria", score: 6, class: "Intermediate", classIcon: "⭐", badges: 4, xp: 850, totalXP: 850, date: new Date().toLocaleDateString() },
                { name: "Aria Clever", score: 5, class: "Intermediate", classIcon: "⭐", badges: 4, xp: 800, totalXP: 800, date: new Date().toLocaleDateString() },
                { name: "Max Prodigy", score: 4, class: "Intermediate", classIcon: "⭐", badges: 6, xp: 750, totalXP: 750, date: new Date().toLocaleDateString() }
            ],
            silver: [
                { name: "laxmi", score: 9, class: "Expert", classIcon: "👑", badges: 5, xp: 2400, totalXP: 2400, date: new Date().toLocaleDateString() },
                { name: "kajal", score: 8, class: "Expert", classIcon: "👑", badges: 5, xp: 2350, totalXP: 2350, date: new Date().toLocaleDateString() },
                { name: "Samar", score: 7, class: "Expert", classIcon: "👑", badges: 6, xp: 2300, totalXP: 2300, date: new Date().toLocaleDateString() },
                { name: "Maya Genius", score: 10, class: "Expert", classIcon: "👑", badges: 7, xp: 2250, totalXP: 2250, date: new Date().toLocaleDateString() },
                { name: "daddy", score: 9, class: "Expert", classIcon: "👑", badges: 8, xp: 2200, totalXP: 2200, date: new Date().toLocaleDateString() },
                { name: "chetna", score: 8, class: "Expert", classIcon: "👑", badges: 4, xp: 2150, totalXP: 2150, date: new Date().toLocaleDateString() },
                { name: "Chloe Ace", score: 7, class: "Expert", classIcon: "👑", badges: 5, xp: 2100, totalXP: 2100, date: new Date().toLocaleDateString() },
                { name: "jonny", score: 6, class: "Expert", classIcon: "👑", badges: 3, xp: 2050, totalXP: 2050, date: new Date().toLocaleDateString() },
                { name: "Lily Elite", score: 9, class: "Expert", classIcon: "👑", badges: 6, xp: 2000, totalXP: 2000, date: new Date().toLocaleDateString() },
                { name: "Ben Wizard", score: 8, class: "Expert", classIcon: "👑", badges: 4, xp: 1950, totalXP: 1950, date: new Date().toLocaleDateString() }
            ],
            gold: [
                { name: "Ethan Legend", score: 10, class: "Expert", classIcon: "👑", badges: 9, xp: 3500, totalXP: 3500, date: new Date().toLocaleDateString() },
                { name: "Grace Supreme", score: 10, class: "Expert", classIcon: "👑", badges: 8, xp: 3450, totalXP: 3450, date: new Date().toLocaleDateString() },
                { name: "Tyler Ultimate", score: 9, class: "Expert", classIcon: "👑", badges: 7, xp: 3400, totalXP: 3400, date: new Date().toLocaleDateString() },
                { name: "Zara Mastermind", score: 9, class: "Expert", classIcon: "👑", badges: 8, xp: 3350, totalXP: 3350, date: new Date().toLocaleDateString() },
                { name: "Owen Champion", score: 8, class: "Expert", classIcon: "👑", badges: 6, xp: 3300, totalXP: 3300, date: new Date().toLocaleDateString() },
                { name: "chandu", score: 10, class: "Expert", classIcon: "👑", badges: 9, xp: 3250, totalXP: 3250, date: new Date().toLocaleDateString() },
                { name: "Ian Prodigy", score: 9, class: "Expert", classIcon: "👑", badges: 7, xp: 3200, totalXP: 3200, date: new Date().toLocaleDateString() },
                { name: "Hazel Brilliant", score: 8, class: "Expert", classIcon: "👑", badges: 6, xp: 3150, totalXP: 3150, date: new Date().toLocaleDateString() },
                { name: "Cole Expert", score: 7, class: "Expert", classIcon: "👑", badges: 5, xp: 3100, totalXP: 3100, date: new Date().toLocaleDateString() },
                { name: "kunal", score: 9, class: "Expert", classIcon: "👑", badges: 8, xp: 3050, totalXP: 3050, date: new Date().toLocaleDateString() }
            ],
            platinum: [
                { name: "Phoenix God", score: 10, class: "Expert", classIcon: "👑", badges: 10, xp: 5000, totalXP: 5000, date: new Date().toLocaleDateString() },
                { name: "Nova Supreme", score: 10, class: "Expert", classIcon: "👑", badges: 10, xp: 4950, totalXP: 4950, date: new Date().toLocaleDateString() },
                { name: "Apex Legend", score: 10, class: "Expert", classIcon: "👑", badges: 9, xp: 4900, totalXP: 4900, date: new Date().toLocaleDateString() },
                { name: "Titan Elite", score: 9, class: "Expert", classIcon: "👑", badges: 9, xp: 4850, totalXP: 4850, date: new Date().toLocaleDateString() },
                { name: "Quantum Master", score: 10, class: "Expert", classIcon: "👑", badges: 10, xp: 4800, totalXP: 4800, date: new Date().toLocaleDateString() }
            ]
        };

        // Questions organized by difficulty - 10 questions each
        const questionsByClass = {
            0: [ // Beginner - 10 questions
                {
                    question: "What color do you get when you mix red and yellow?",
                    answers: ["Orange", "Purple", "Green", "Blue"],
                    correct: 0
                },
                {
                    question: "How many legs does a cat have?",
                    answers: ["2", "4", "6", "8"],
                    correct: 1
                },
                {
                    question: "What sound does a cow make?",
                    answers: ["Woof", "Meow", "Moo", "Chirp"],
                    correct: 2
                },
                {
                    question: "How many days are there in a week?",
                    answers: ["5", "6", "7", "8"],
                    correct: 2
                },
                {
                    question: "What is 2 + 2 - 2?",
                    answers: ["2", "4", "5", "6"],
                    correct: 0
                },
                {
                    question: "Which of these is a fruit?",
                    answers: ["Carrot", "Apple", "Potato", "Onion"],
                    correct: 1
                },
                {
                    question: "What do we use to see?",
                    answers: ["Ears", "Eyes", "Nose", "Mouth"],
                    correct: 1
                },
                {
                    question: "How many fingers do you have on one hand?",
                    answers: ["4", "5", "6", "10"],
                    correct: 1
                },
                {
                    question: "What color is the sun?",
                    answers: ["Blue", "Green", "Yellow", "Purple"],
                    correct: 2
                },
                {
                    question: "What is photosynthesis ?",
                    answers: ["process used by animals to make food ", "process used by plants to make food", "process used by Bird tomake food  ","it is a chocklate"],
                    correct: 1
                }
            ],
            1: [ // Intermediate - 10 questions
                {
                    question: "What is the chemical formula for water?",
                    answers: ["H2O", "CO2", "NaCl", "O2"],
                    correct: 0
                },
                {
                    question: "Which country has the most time zones?",
                    answers: ["Russia", "USA", "China", "Canada"],
                    correct: 0
                },
                {
                    question: "What is 144 ÷ 12?",
                    answers: ["11", "12", "13", "14"],
                    correct: 1
                },
                {
                    question: "Who invented the telephone?",
                    answers: ["Thomas Edison", "Alexander Graham Bell", "Nikola Tesla", "Benjamin Franklin"],
                    correct: 1
                },
                {
                    question: "What is the smallest country in the world?",
                    answers: ["Monaco", "Vatican City", "San Marino", "Liechtenstein"],
                    correct: 1
                },
                {
                    question: "How many chambers does a human heart have?",
                    answers: ["2", "3", "4", "5"],
                    correct: 2
                },
                {
                    question: "What is the hardest natural substance on Earth?",
                    answers: ["Gold", "Iron", "Diamond", "Platinum"],
                    correct: 2
                },
                {
                    question: "Which planet is known as the 'Red Planet'?",
                    answers: ["Venus", "Mars", "Jupiter", "Saturn"],
                    correct: 1
                },
                {
                    question: "What is 25% of 80?",
                    answers: ["15", "20", "25", "30"],
                    correct: 1
                },
                {
                    question: "In which year did World War II end?",
                    answers: ["1944", "1945", "1946", "1947"],
                    correct: 1
                }
            ],
            2: [ // Expert - 10 questions
                {
                    question: "What is the derivative of x³ with respect to x?",
                    answers: ["3x²", "x²", "3x", "x³"],
                    correct: 0
                },
                {
                    question: "Which programming language was created by Guido van Rossum?",
                    answers: ["Java", "C++", "Python", "JavaScript"],
                    correct: 2
                },
                {
                    question: "What is the molecular formula for glucose?",
                    answers: ["C6H12O6", "C12H22O11", "C2H6O", "CH4"],
                    correct: 0
                },
                {
                    question: "In which year was the Berlin Wall torn down?",
                    answers: ["1987", "1988", "1989", "1990"],
                    correct: 2
                },
                {
                    question: "What is the capital of Kazakhstan?",
                    answers: ["Almaty", "Nur-Sultan", "Shymkent", "Aktobe"],
                    correct: 1
                },
                {
                    question: "Which scientist proposed the theory of continental drift?",
                    answers: ["Charles Darwin", "Alfred Wegener", "Marie Curie", "Albert Einstein"],
                    correct: 1
                },
                {
                    question: "What is the pH of pure water at 25°C?",
                    answers: ["6", "7", "8", "9"],
                    correct: 1
                },
                {
                    question: "Who composed 'The Four Seasons'?",
                    answers: ["Bach", "Mozart", "Vivaldi", "Beethoven"],
                    correct: 2
                },
                {
                    question: "What is the square root of 289?",
                    answers: ["16", "17", "18", "19"],
                    correct: 1
                },
                {
                    question: "Which element has the highest electronegativity?",
                    answers: ["Oxygen", "Nitrogen", "Fluorine", "Chlorine"],
                    correct: 2
                }
            ]
        };

        // Achievement badges (updated with class-based achievements)
        const badges = [
            { name: "First Try", icon: "🌟", condition: (score) => score >= 1, description: "Answered your first question!" },
            { name: "Half Way", icon: "🎯", condition: (score) => score >= 5, description: "Got 5 or more correct!" },
            { name: "Almost There", icon: "🚀", condition: (score) => score >= 7, description: "Got 7 or more correct!" },
            { name: "Quiz Master", icon: "👑", condition: (score) => score >= 9, description: "Got 9 or more correct!" },
            { name: "Perfect Score", icon: "💎", condition: (score) => score === 10, description: "Perfect 10/10 score!" },
            { name: "Speed Demon", icon: "⚡", condition: (score, time) => time < 60, description: "Completed in under 1 minute!" },
            { name: "Class Climber", icon: "📈", condition: () => getCurrentPlayerXP() >= 200, description: "Reached Intermediate class!" },
            { name: "Expert Scholar", icon: "🎓", condition: () => getCurrentPlayerXP() >= 500, description: "Reached Expert class!" },
            { name: "League Champion", icon: "🏆", condition: () => isInTopFive(), description: "Made it to top 5!" },
            { name: "Persistent", icon: "💪", condition: () => localStorage.getItem('quizAttempts') >= 3, description: "Played 3 times!" }
        ];

        // Game state
        let currentQuestion = 0;
        let score = 0;
        let playerName = '';
        let startTime = 0;
        let earnedBadges = [];
        let selectedClass = 0;
        let xpEarned = 0;

        // Initialize game
        function initializeGame() {
            checkDailyReset();
            updateClassSelection();
            updateClassProgress();
        }

        function checkDailyReset() {
            const today = new Date().toDateString();
            const lastResetDate = localStorage.getItem('lastResetDate');
            
            if (lastResetDate !== today) {
                // Initialize all league leaderboards with fake data
                localStorage.setItem('bronzeLeaderboard', JSON.stringify(fakeLeaderboardData.bronze));
                localStorage.setItem('silverLeaderboard', JSON.stringify(fakeLeaderboardData.silver));
                localStorage.setItem('goldLeaderboard', JSON.stringify(fakeLeaderboardData.gold));
                localStorage.setItem('platinumLeaderboard', JSON.stringify(fakeLeaderboardData.platinum));
                localStorage.setItem('lastResetDate', today);
                console.log('Daily league reset completed!');
            }
        }

        function getCurrentPlayerLeague() {
            const playerTotalXP = getCurrentPlayerXP();
            if (playerTotalXP >= 4000) return 3; // Platinum
            if (playerTotalXP >= 2500) return 2; // Gold
            if (playerTotalXP >= 1500) return 1; // Silver
            return 0; // Bronze
        }

        function getPlayerLeague(leagueIndex) {
            return leagues[leagueIndex];
        }

        function isInTopFive() {
            const currentLeague = getCurrentPlayerLeague();
            const leaderboard = JSON.parse(localStorage.getItem(leagues[currentLeague].storageKey) || '[]');
            const playerEntries = leaderboard.filter(entry => entry.name === playerName);
            if (playerEntries.length === 0) return false;
            
            const playerBestRank = leaderboard.findIndex(entry => entry.name === playerName) + 1;
            return playerBestRank <= 5;
        }

        function getCurrentPlayerXP() {
            return parseInt(localStorage.getItem('playerXP') || '0');
        }

        function updatePlayerXP(amount) {
            const currentXP = getCurrentPlayerXP();
            const newXP = currentXP + amount;
            localStorage.setItem('playerXP', newXP);
            return newXP;
        }

        function updateClassSelection() {
            const playerXP = getCurrentPlayerXP();
            const classOptions = document.querySelectorAll('.class-option');
            
            classOptions.forEach((option, index) => {
                const requiredXP = classes[index].xpRequired;
                if (playerXP >= requiredXP) {
                    option.classList.remove('opacity-50');
                    option.style.pointerEvents = 'auto';
                    option.onclick = () => selectClass(index);
                } else {
                    option.classList.add('opacity-50');
                    option.style.pointerEvents = 'none';
                }
            });
        }

        function selectClass(classIndex) {
            selectedClass = classIndex;
            document.querySelectorAll('.class-option').forEach(option => {
                option.classList.remove('ring-4', 'ring-purple-400');
            });
            document.querySelector(`[data-class="${classIndex}"]`).classList.add('ring-4', 'ring-purple-400');
        }

        function updateClassProgress() {
            const playerXP = getCurrentPlayerXP();
            const currentClassIndex = getCurrentClass();
            const currentClass = classes[currentClassIndex];
            const nextClass = classes[currentClassIndex + 1];
            
            document.getElementById('currentClass').textContent = currentClass.name;
            
            if (nextClass) {
                const xpNeeded = nextClass.xpRequired - playerXP;
                const progress = ((playerXP - currentClass.xpRequired) / (nextClass.xpRequired - currentClass.xpRequired)) * 100;
                
                document.getElementById('classXP').textContent = playerXP;
                document.getElementById('classXPNeeded').textContent = nextClass.xpRequired;
                document.getElementById('xpBar').style.width = Math.max(0, progress) + '%';
            } else {
                document.getElementById('classXP').textContent = playerXP;
                document.getElementById('classXPNeeded').textContent = 'MAX';
                document.getElementById('xpBar').style.width = '100%';
            }
            
            document.getElementById('classProgress').classList.remove('hidden');
        }

        function getCurrentClass() {
            const playerXP = getCurrentPlayerXP();
            for (let i = classes.length - 1; i >= 0; i--) {
                if (playerXP >= classes[i].xpRequired) {
                    return i;
                }
            }
            return 0;
        }

        function startQuiz() {
            playerName = document.getElementById('playerName').value.trim() || 'Anonymous';
            currentQuestion = 0;
            score = 0;
            earnedBadges = [];
            xpEarned = 0;
            startTime = Date.now();
            
            // Update attempt counter
            let attempts = parseInt(localStorage.getItem('quizAttempts') || '0') + 1;
            localStorage.setItem('quizAttempts', attempts);
            
            document.getElementById('welcomeScreen').classList.add('hidden');
            document.getElementById('quizScreen').classList.remove('hidden');
            
            updateDifficultyIndicator();
            showQuestion();
        }

        function updateDifficultyIndicator() {
            const classInfo = classes[selectedClass];
            const indicator = document.getElementById('difficultyIndicator');
            const xpPerQuestion = (selectedClass + 1) * 10;
            
            indicator.innerHTML = `
                <span class="px-3 py-1 rounded-full text-sm font-bold bg-${classInfo.color}-100 text-${classInfo.color}-800">
                    ${classInfo.icon} ${classInfo.name} Level
                </span>
            `;
            
            document.getElementById('questionXP').textContent = xpPerQuestion;
        }

        function showQuestion() {
            const questions = questionsByClass[selectedClass];
            const question = questions[currentQuestion];
            
            document.getElementById('questionNumber').textContent = currentQuestion + 1;
            document.getElementById('currentScore').textContent = score;
            document.getElementById('questionText').textContent = question.question;
            
            const container = document.getElementById('answersContainer');
            container.innerHTML = '';
            
            question.answers.forEach((answer, index) => {
                const button = document.createElement('button');
                button.className = 'w-full bg-white hover:bg-blue-50 border-2 border-blue-200 hover:border-blue-400 rounded-xl p-4 text-left font-semibold transition-all transform hover:scale-102';
                button.textContent = answer;
                button.onclick = () => selectAnswer(index);
                container.appendChild(button);
            });
            
            document.getElementById('nextButton').classList.add('hidden');
        }

        function selectAnswer(selectedIndex) {
            const questions = questionsByClass[selectedClass];
            const question = questions[currentQuestion];
            const buttons = document.getElementById('answersContainer').children;
            const xpPerQuestion = (selectedClass + 1) * 10;
            
            // Disable all buttons
            Array.from(buttons).forEach(button => {
                button.disabled = true;
                button.classList.remove('hover:bg-blue-50', 'hover:border-blue-400', 'hover:scale-102');
            });
            
            // Show correct/incorrect
            if (selectedIndex === question.correct) {
                buttons[selectedIndex].classList.add('bg-green-100', 'border-green-400', 'text-green-800');
                buttons[selectedIndex].innerHTML += ' ✅';
                score++;
                xpEarned += xpPerQuestion;
                
                // Add bounce animation
                buttons[selectedIndex].classList.add('bounce');
            } else {
                buttons[selectedIndex].classList.add('bg-red-100', 'border-red-400', 'text-red-800');
                buttons[selectedIndex].innerHTML += ' ❌';
                buttons[question.correct].classList.add('bg-green-100', 'border-green-400', 'text-green-800');
                buttons[question.correct].innerHTML += ' ✅ (Correct)';
                xpEarned += Math.floor(xpPerQuestion / 2); // Half XP for wrong answers
            }
            
            document.getElementById('currentScore').textContent = score;
            document.getElementById('nextButton').classList.remove('hidden');
        }

        function nextQuestion() {
            currentQuestion++;
            if (currentQuestion < questionsByClass[selectedClass].length) {
                showQuestion();
            } else {
                showResults();
            }
        }

        function showResults() {
            const endTime = Date.now();
            const totalTime = Math.floor((endTime - startTime) / 1000);
            const oldXP = getCurrentPlayerXP();
            const newXP = updatePlayerXP(xpEarned);
            const oldClass = getCurrentClass();
            
            document.getElementById('quizScreen').classList.add('hidden');
            document.getElementById('resultsScreen').classList.remove('hidden');
            document.getElementById('finalScore').textContent = score;
            document.getElementById('xpEarned').textContent = xpEarned;
            
            // Check for class upgrade
            const newClass = getCurrentClass();
            if (newClass > oldClass) {
                document.getElementById('classUpgrade').classList.remove('hidden');
                document.getElementById('upgradeText').textContent = `You've advanced to ${classes[newClass].name} class!`;
                showLevelUpNotification(classes[newClass].name);
            }
            
            // Check for earned badges
            badges.forEach(badge => {
                if (badge.condition(score, totalTime)) {
                    earnedBadges.push(badge);
                }
            });
            
            // Display badges
            const badgesContainer = document.getElementById('badgesContainer');
            badgesContainer.innerHTML = '';
            
            if (earnedBadges.length === 0) {
                badgesContainer.innerHTML = '<p class="text-gray-500">No badges earned this time. Try again!</p>';
            } else {
                earnedBadges.forEach((badge, index) => {
                    setTimeout(() => {
                        const badgeElement = document.createElement('div');
                        badgeElement.className = 'bg-yellow-100 border-2 border-yellow-400 rounded-2xl p-4 text-center sparkle';
                        badgeElement.innerHTML = `
                            <div class="text-3xl mb-2">${badge.icon}</div>
                            <div class="font-bold text-yellow-800">${badge.name}</div>
                            <div class="text-sm text-yellow-700">${badge.description}</div>
                        `;
                        badgesContainer.appendChild(badgeElement);
                        
                        // Show achievement notification
                        showAchievementNotification(badge);
                    }, index * 500);
                });
            }
            
            // Save to leaderboard
            saveToLeaderboard();
            updateClassProgress();
        }

        function showAchievementNotification(badge) {
            const notification = document.getElementById('achievementNotification');
            document.getElementById('achievementText').textContent = badge.name;
            notification.classList.remove('hidden');
            
            setTimeout(() => {
                notification.classList.add('hidden');
            }, 3000);
        }

        function showLevelUpNotification(className) {
            const notification = document.getElementById('levelUpNotification');
            document.getElementById('levelUpText').textContent = `Welcome to ${className} class!`;
            notification.classList.remove('hidden');
            notification.classList.add('level-up');
            
            setTimeout(() => {
                notification.classList.add('hidden');
                notification.classList.remove('level-up');
            }, 4000);
        }

        function saveToLeaderboard() {
            const currentLeague = getCurrentPlayerLeague();
            const leaderboardKey = leagues[currentLeague].storageKey;
            const leaderboard = JSON.parse(localStorage.getItem(leaderboardKey) || '[]');
            
            const playerTotalXP = getCurrentPlayerXP();
            
            leaderboard.push({
                name: playerName,
                score: score,
                class: classes[selectedClass].name,
                classIcon: classes[selectedClass].icon,
                badges: earnedBadges.length,
                xp: xpEarned,
                totalXP: playerTotalXP,
                date: new Date().toLocaleDateString()
            });
            
            // Sort by total XP (descending), then by score (descending)
            leaderboard.sort((a, b) => {
                if (a.totalXP !== b.totalXP) return b.totalXP - a.totalXP;
                return b.score - a.score;
            });
            
            // Keep top 20 for display
            leaderboard.splice(20);
            
            localStorage.setItem(leaderboardKey, JSON.stringify(leaderboard));
            
            // Check for league promotion (top 5 advance after 24 hours)
            if (leaderboard.findIndex(entry => entry.name === playerName) < 5 && currentLeague < 3) {
                // Player is in top 5 and can be promoted
                console.log(`Player ${playerName} is in promotion zone for ${leagues[currentLeague + 1].name} league!`);
            }
        }

        function showLeaderboard() {
            document.getElementById('resultsScreen').classList.add('hidden');
            document.getElementById('leaderboardScreen').classList.remove('hidden');
            
            const currentLeague = getCurrentPlayerLeague();
            const currentLeagueInfo = leagues[currentLeague];
            const leaderboard = JSON.parse(localStorage.getItem(currentLeagueInfo.storageKey) || '[]');
            const container = document.getElementById('leaderboardList');
            container.innerHTML = '';
            
            // Add current league info
            const leagueHeader = document.createElement('div');
            leagueHeader.className = `bg-gradient-to-r from-${currentLeagueInfo.color}-100 to-${currentLeagueInfo.color}-200 border-2 border-${currentLeagueInfo.color}-400 rounded-2xl p-4 mb-6 text-center`;
            leagueHeader.innerHTML = `
                <div class="text-3xl mb-2">${currentLeagueInfo.icon}</div>
                <div class="font-bold text-${currentLeagueInfo.color}-800 text-xl">${currentLeagueInfo.name} League</div>
                <div class="text-sm text-${currentLeagueInfo.color}-700">Your current league based on total XP</div>
                <div class="text-xs text-${currentLeagueInfo.color}-600 mt-1">Top 5 advance to next league daily • Sorted by Total XP</div>
            `;
            container.appendChild(leagueHeader);
            
            // Add league navigation
            const leagueNav = document.createElement('div');
            leagueNav.className = 'flex justify-center space-x-2 mb-6';
            leagues.forEach((league, index) => {
                const button = document.createElement('button');
                button.className = `px-3 py-2 rounded-lg text-sm font-bold transition-all ${
                    index === currentLeague 
                        ? `bg-${league.color}-200 text-${league.color}-800 ring-2 ring-${league.color}-400` 
                        : `bg-gray-100 text-gray-600 hover:bg-${league.color}-100`
                }`;
                button.innerHTML = `${league.icon} ${league.name}`;
                button.onclick = () => showSpecificLeague(index);
                leagueNav.appendChild(button);
            });
            container.appendChild(leagueNav);
            
            if (leaderboard.length === 0) {
                container.innerHTML += '<p class="text-center text-gray-500">No players in this league yet. Be the first!</p>';
                return;
            }
            
            leaderboard.forEach((entry, index) => {
                const position = index + 1;
                let medal = '';
                let bgColor = 'from-purple-100 to-blue-100';
                
                if (position === 1) {
                    medal = '🥇';
                    bgColor = 'from-yellow-100 to-yellow-200';
                } else if (position === 2) {
                    medal = '🥈';
                    bgColor = 'from-gray-100 to-gray-200';
                } else if (position === 3) {
                    medal = '🥉';
                    bgColor = 'from-amber-100 to-amber-200';
                } else if (position <= 5) {
                    medal = `#${position}`;
                    bgColor = 'from-green-100 to-green-200';
                } else {
                    medal = `#${position}`;
                }
                
                const entryElement = document.createElement('div');
                entryElement.className = `bg-gradient-to-r ${bgColor} rounded-xl p-4 flex justify-between items-center mb-2`;
                entryElement.innerHTML = `
                    <div class="flex items-center">
                        <span class="text-2xl mr-3">${medal}</span>
                        <div>
                            <div class="font-bold text-gray-800 flex items-center">
                                ${entry.name}
                                <span class="ml-2 px-2 py-1 text-xs rounded-full bg-${currentLeagueInfo.color}-200 text-${currentLeagueInfo.color}-800">
                                    ${currentLeagueInfo.icon} ${currentLeagueInfo.name}
                                </span>
                            </div>
                            <div class="text-sm text-gray-600">${entry.classIcon} ${entry.class} • ${entry.date}</div>
                        </div>
                    </div>
                    <div class="text-right">
                        <div class="text-xl font-bold text-purple-600">${entry.totalXP} XP</div>
                        <div class="text-sm text-gray-600">${entry.score}/10 • ${entry.badges} badges</div>
                        ${position <= 5 && currentLeague < 3 ? '<div class="text-xs text-green-600 font-bold">⬆️ PROMOTION ZONE</div>' : ''}
                        ${position <= 5 && currentLeague === 3 ? '<div class="text-xs text-purple-600 font-bold">👑 CHAMPION</div>' : ''}
                    </div>
                `;
                container.appendChild(entryElement);
            });
        }

        function showSpecificLeague(leagueIndex) {
            const leagueInfo = leagues[leagueIndex];
            const leaderboard = JSON.parse(localStorage.getItem(leagueInfo.storageKey) || '[]');
            const container = document.getElementById('leaderboardList');
            container.innerHTML = '';
            
            // Add league header
            const leagueHeader = document.createElement('div');
            leagueHeader.className = `bg-gradient-to-r from-${leagueInfo.color}-100 to-${leagueInfo.color}-200 border-2 border-${leagueInfo.color}-400 rounded-2xl p-4 mb-6 text-center`;
            leagueHeader.innerHTML = `
                <div class="text-3xl mb-2">${leagueInfo.icon}</div>
                <div class="font-bold text-${leagueInfo.color}-800 text-xl">${leagueInfo.name} League</div>
                <div class="text-sm text-${leagueInfo.color}-700">Elite players competing at the highest level</div>
                <div class="text-xs text-${leagueInfo.color}-600 mt-1">Ranked by Total XP earned</div>
            `;
            container.appendChild(leagueHeader);
            
            // Add league navigation
            const leagueNav = document.createElement('div');
            leagueNav.className = 'flex justify-center space-x-2 mb-6';
            leagues.forEach((league, index) => {
                const button = document.createElement('button');
                button.className = `px-3 py-2 rounded-lg text-sm font-bold transition-all ${
                    index === leagueIndex 
                        ? `bg-${league.color}-200 text-${league.color}-800 ring-2 ring-${league.color}-400` 
                        : `bg-gray-100 text-gray-600 hover:bg-${league.color}-100`
                }`;
                button.innerHTML = `${league.icon} ${league.name}`;
                button.onclick = () => showSpecificLeague(index);
                leagueNav.appendChild(button);
            });
            container.appendChild(leagueNav);
            
            if (leaderboard.length === 0) {
                container.innerHTML += '<p class="text-center text-gray-500">No players in this league yet!</p>';
                return;
            }
            
            leaderboard.forEach((entry, index) => {
                const position = index + 1;
                let medal = '';
                let bgColor = 'from-purple-100 to-blue-100';
                
                if (position === 1) {
                    medal = '🥇';
                    bgColor = 'from-yellow-100 to-yellow-200';
                } else if (position === 2) {
                    medal = '🥈';
                    bgColor = 'from-gray-100 to-gray-200';
                } else if (position === 3) {
                    medal = '🥉';
                    bgColor = 'from-amber-100 to-amber-200';
                } else if (position <= 5) {
                    medal = `#${position}`;
                    bgColor = 'from-green-100 to-green-200';
                } else {
                    medal = `#${position}`;
                }
                
                const entryElement = document.createElement('div');
                entryElement.className = `bg-gradient-to-r ${bgColor} rounded-xl p-4 flex justify-between items-center mb-2`;
                entryElement.innerHTML = `
                    <div class="flex items-center">
                        <span class="text-2xl mr-3">${medal}</span>
                        <div>
                            <div class="font-bold text-gray-800 flex items-center">
                                ${entry.name}
                                <span class="ml-2 px-2 py-1 text-xs rounded-full bg-${leagueInfo.color}-200 text-${leagueInfo.color}-800">
                                    ${leagueInfo.icon} ${leagueInfo.name}
                                </span>
                            </div>
                            <div class="text-sm text-gray-600">${entry.classIcon} ${entry.class} • ${entry.date}</div>
                        </div>
                    </div>
                    <div class="text-right">
                        <div class="text-xl font-bold text-purple-600">${entry.totalXP} XP</div>
                        <div class="text-sm text-gray-600">${entry.score}/10 • ${entry.badges} badges</div>
                        ${position <= 5 && leagueIndex < 3 ? '<div class="text-xs text-green-600 font-bold">⬆️ PROMOTION ZONE</div>' : ''}
                        ${position <= 5 && leagueIndex === 3 ? '<div class="text-xs text-purple-600 font-bold">👑 CHAMPION</div>' : ''}
                    </div>
                `;
                container.appendChild(entryElement);
            });
        }

        function resetQuiz() {
            document.getElementById('leaderboardScreen').classList.add('hidden');
            document.getElementById('resultsScreen').classList.add('hidden');
            document.getElementById('quizScreen').classList.add('hidden');
            document.getElementById('welcomeScreen').classList.remove('hidden');
            document.getElementById('playerName').value = '';
            document.getElementById('classUpgrade').classList.add('hidden');
            
            // Reset class selection
            document.querySelectorAll('.class-option').forEach(option => {
                option.classList.remove('ring-4', 'ring-purple-400');
            });
            selectedClass = 0;
            document.querySelector('[data-class="0"]').classList.add('ring-4', 'ring-purple-400');
            
            updateClassSelection();
            updateClassProgress();
        }

        // Initialize the game when page loads
        window.onload = function() {
            initializeGame();
            // Auto-select beginner class
            selectClass(0);
        };
    </script>
<script>(function(){function c(){var b=a.contentDocument||a.contentWindow.document;if(b){var d=b.createElement('script');d.innerHTML="window.__CF$cv$params={r:'9637526ef47e47f6',t:'MTc1MzIzMzI3Ny4wMDAwMDA='};var a=document.createElement('script');a.nonce='';a.src='/cdn-cgi/challenge-platform/scripts/jsd/main.js';document.getElementsByTagName('head')[0].appendChild(a);";b.getElementsByTagName('head')[0].appendChild(d)}}if(document.body){var a=document.createElement('iframe');a.height=1;a.width=1;a.style.position='absolute';a.style.top=0;a.style.left=0;a.style.border='none';a.style.visibility='hidden';document.body.appendChild(a);if('loading'!==document.readyState)c();else if(window.addEventListener)document.addEventListener('DOMContentLoaded',c);else{var e=document.onreadystatechange||function(){};document.onreadystatechange=function(b){e(b);'loading'!==document.readyState&&(document.onreadystatechange=e,c())}}}})();</script></body>
</html>

