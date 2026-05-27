```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LeetCode Lite - Interactive Coding Platform</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <!-- Monaco Editor (VS Code Editor for Web) -->
    <link rel="stylesheet" data-name="vs/editor/editor.main" href="https://cdnjs.cloudflare.com/ajax/libs/monaco-editor/0.39.0/min/vs/editor/editor.main.min.css">
    <style>
        /* Custom scrollbar for premium dark theme */
        ::-webkit-scrollbar {
            width: 8px;
            height: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #1e1e1e;
        }
        ::-webkit-scrollbar-thumb {
            background: #333;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #444;
        }
        .monaco-editor {
            padding-top: 8px;
        }
    </style>
</head>
<body class="bg-[#1a1a1a] text-gray-200 font-sans min-h-screen flex flex-col">

    <!-- Header Navigation -->
    <header class="border-b border-[#333333] bg-[#0a0a0a] px-6 py-3 flex items-center justify-between sticky top-0 z-50">
        <div class="flex items-center space-x-6">
            <div class="flex items-center space-x-2">
                <div class="bg-yellow-500 text-black p-1.5 rounded-lg font-bold flex items-center justify-center">
                    <i data-lucide="code-2" class="w-5 h-5"></i>
                </div>
                <span class="font-bold text-xl tracking-tight text-white">Leet<span class="text-yellow-500">Code</span> <span class="text-xs px-2 py-0.5 rounded bg-zinc-800 text-zinc-400 font-normal">Lite</span></span>
            </div>
            <nav class="hidden md:flex space-x-4 text-sm font-medium text-gray-400">
                <button onclick="switchTab('problems')" class="hover:text-white px-3 py-1.5 rounded transition nav-btn active text-yellow-500">Problems</button>
                <button onclick="switchTab('playground')" class="hover:text-white px-3 py-1.5 rounded transition nav-btn">Playground</button>
            </nav>
        </div>
        <div class="flex items-center space-x-4">
            <div class="text-xs bg-zinc-800 text-green-400 px-3 py-1.5 rounded-full flex items-center space-x-1">
                <span class="w-2 h-2 rounded-full bg-green-500 animate-pulse"></span>
                <span>AI Compiler Active</span>
            </div>
        </div>
    </header>

    <!-- Main Content Area -->
    <main class="flex-1 flex overflow-hidden">
        
        <!-- PROBLEMS LIST PAGE -->
        <div id="problems-view" class="w-full max-w-6xl mx-auto px-4 py-8 overflow-y-auto">
            <div class="mb-8">
                <h1 class="text-2xl font-bold text-white mb-2">Explore Coding Problems</h1>
                <p class="text-gray-400 text-sm">Practice your coding skills with instant AI feedback on code correctness and efficiency.</p>
            </div>

            <!-- Problem Statistics cards -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-8">
                <div class="bg-zinc-900 border border-zinc-800 p-4 rounded-xl flex items-center justify-between">
                    <div>
                        <p class="text-xs text-gray-400 uppercase tracking-wider">Total Challenges</p>
                        <h3 class="text-2xl font-bold text-white mt-1" id="total-problems-count">4</h3>
                    </div>
                    <div class="p-3 bg-blue-500/10 text-blue-400 rounded-lg">
                        <i data-lucide="database" class="w-6 h-6"></i>
                    </div>
                </div>
                <div class="bg-zinc-900 border border-zinc-800 p-4 rounded-xl flex items-center justify-between">
                    <div>
                        <p class="text-xs text-gray-400 uppercase tracking-wider">Passed Submissions</p>
                        <h3 class="text-2xl font-bold text-green-400 mt-1" id="passed-count">0</h3>
                    </div>
                    <div class="p-3 bg-green-500/10 text-green-400 rounded-lg">
                        <i data-lucide="check-circle" class="w-6 h-6"></i>
                    </div>
                </div>
                <div class="bg-zinc-900 border border-zinc-800 p-4 rounded-xl flex items-center justify-between">
                    <div>
                        <p class="text-xs text-gray-400 uppercase tracking-wider">Tackled</p>
                        <h3 class="text-2xl font-bold text-yellow-500 mt-1" id="attempted-count">0</h3>
                    </div>
                    <div class="p-3 bg-yellow-500/10 text-yellow-500 rounded-lg">
                        <i data-lucide="award" class="w-6 h-6"></i>
                    </div>
                </div>
            </div>

            <!-- Problem Table -->
            <div class="bg-zinc-900 border border-zinc-800 rounded-xl overflow-hidden">
                <div class="overflow-x-auto">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="border-b border-zinc-800 text-xs font-semibold uppercase tracking-wider text-gray-400 bg-zinc-950">
                                <th class="py-4 px-6">Status</th>
                                <th class="py-4 px-6">Title</th>
                                <th class="py-4 px-6">Difficulty</th>
                                <th class="py-4 px-6">Category</th>
                                <th class="py-4 px-6 text-right">Action</th>
                            </tr>
                        </thead>
                        <tbody id="problems-table-body" class="divide-y divide-zinc-800 text-sm">
                            <!-- Injected dynamically via JS -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- PLAYGROUND / CODE WORKSPACE (SPLIT SCREEN) -->
        <div id="workspace-view" class="hidden w-full flex flex-col md:flex-row overflow-hidden h-[calc(100vh-57px)]">
            
            <!-- Left Panel: Problem Description -->
            <div class="w-full md:w-5/12 border-r border-[#333333] flex flex-col bg-[#141414] h-1/2 md:h-full">
                <!-- Tabs for Problem panel -->
                <div class="border-b border-[#333333] flex items-center justify-between px-4 bg-[#0d0d0d]">
                    <div class="flex space-x-2">
                        <button class="px-4 py-3 border-b-2 border-yellow-500 text-white text-sm font-medium">Description</button>
                    </div>
                    <button onclick="switchTab('problems')" class="text-gray-400 hover:text-white flex items-center text-xs space-x-1">
                        <i data-lucide="arrow-left" class="w-4 h-4"></i>
                        <span>Back to problems</span>
                    </button>
                </div>
                <!-- Problem Details Content -->
                <div class="flex-1 overflow-y-auto p-6 space-y-6">
                    <div>
                        <span id="workspace-difficulty" class="text-xs px-2.5 py-1 rounded-full font-semibold">Easy</span>
                        <h2 id="workspace-title" class="text-xl font-bold text-white mt-3">Two Sum</h2>
                    </div>

                    <!-- Problem statement -->
                    <div class="text-gray-300 text-sm leading-relaxed space-y-4" id="workspace-statement">
                        <!-- Injected via JS -->
                    </div>

                    <!-- Constraints -->
                    <div class="space-y-2">
                        <h4 class="text-xs font-semibold uppercase text-gray-400 tracking-wider">Constraints:</h4>
                        <ul id="workspace-constraints" class="list-disc pl-5 text-xs text-gray-400 space-y-1">
                            <!-- Injected via JS -->
                        </ul>
                    </div>

                    <!-- Input / Output Examples -->
                    <div class="space-y-4" id="workspace-examples">
                        <!-- Injected via JS -->
                    </div>
                </div>
            </div>

            <!-- Right Panel: Editor + Output Terminal -->
            <div class="w-full md:w-7/12 flex flex-col h-1/2 md:h-full">
                
                <!-- Editor Header -->
                <div class="border-b border-[#333333] px-4 py-2 bg-[#0d0d0d] flex items-center justify-between">
                    <div class="flex items-center space-x-2">
                        <i data-lucide="code" class="text-yellow-500 w-4 h-4"></i>
                        <select id="language-select" class="bg-zinc-800 text-white border border-zinc-700 text-xs rounded px-2.5 py-1 focus:outline-none focus:border-yellow-500">
                            <option value="python">Python 3</option>
                            <option value="javascript">JavaScript</option>
                            <option value="cpp">C++</option>
                            <option value="java">Java</option>
                        </select>
                    </div>
                    <button id="reset-code-btn" class="text-xs text-gray-400 hover:text-white flex items-center space-x-1 px-2 py-1 rounded hover:bg-zinc-800 transition">
                        <i data-lucide="rotate-ccw" class="w-3.5 h-3.5"></i>
                        <span>Reset</span>
                    </button>
                </div>

                <!-- Monaco Editor Container -->
                <div class="flex-1 bg-[#1e1e1e] relative min-h-[250px]">
                    <div id="monaco-container" class="absolute inset-0"></div>
                </div>

                <!-- Interactive Test Terminal Panel -->
                <div class="border-t border-[#333333] bg-[#0d0d0d] flex flex-col h-[280px]">
                    <!-- Terminal Tab Bar -->
                    <div class="border-b border-[#333333] px-4 py-2 flex items-center justify-between">
                        <div class="flex space-x-4 text-xs font-medium">
                            <button onclick="switchTerminalTab('testcase')" id="tab-testcase" class="text-yellow-500 border-b-2 border-yellow-500 pb-1.5">Testcase Customizer</button>
                            <button onclick="switchTerminalTab('result')" id="tab-result" class="text-gray-400 hover:text-white pb-1.5">Submission Result</button>
                        </div>
                    </div>

                    <!-- Terminal Tab Content Area -->
                    <div class="flex-1 overflow-y-auto p-4 font-mono text-xs">
                        <!-- Custom Test Cases Tab -->
                        <div id="terminal-testcase-content" class="space-y-3">
                            <p class="text-gray-400">Specify custom input to test your logic before full submission:</p>
                            <textarea id="custom-input-box" class="w-full h-24 bg-[#1a1a1a] border border-[#333333] rounded p-2.5 text-gray-300 focus:outline-none focus:border-yellow-500 placeholder-zinc-600" placeholder="e.g. nums = [2, 7, 11, 15], target = 9"></textarea>
                        </div>

                        <!-- Submission Results Tab -->
                        <div id="terminal-result-content" class="hidden h-full flex flex-col justify-between">
                            <div id="result-status-card" class="bg-zinc-900 border border-zinc-800 p-4 rounded-lg hidden">
                                <div class="flex items-start space-x-3">
                                    <div id="result-badge-container">
                                        <!-- Dynamic badge -->
                                    </div>
                                    <div class="flex-1">
                                        <div id="result-status-title" class="font-bold text-sm">Waiting for Run/Submit...</div>
                                        <div id="result-details" class="text-gray-400 mt-2 space-y-2 whitespace-pre-line text-[11px] leading-relaxed">
                                            Run your code to see outputs and execution details.
                                        </div>
                                    </div>
                                </div>
                            </div>
                            <!-- Initial Placeholder -->
                            <div id="result-placeholder" class="text-center text-gray-500 py-12">
                                <i data-lucide="terminal" class="w-8 h-8 mx-auto mb-2 text-zinc-600"></i>
                                <p>Execute 'Run Code' or 'Submit' to run unit tests & generate feedback.</p>
                            </div>
                        </div>
                    </div>

                    <!-- Action Buttons -->
                    <div class="border-t border-[#333333] px-4 py-3 flex justify-between bg-[#111] items-center">
                        <div class="text-xs text-gray-500 flex items-center space-x-1.5">
                            <i data-lucide="cpu" class="w-3.5 h-3.5"></i>
                            <span>Powered by Gemini 2.5 Flash</span>
                        </div>
                        <div class="flex space-x-2">
                            <button id="run-code-btn" class="bg-zinc-800 text-gray-200 border border-zinc-700 hover:bg-zinc-700 font-semibold px-4 py-1.5 rounded-lg text-xs transition flex items-center space-x-1.5">
                                <i data-lucide="play" class="w-3.5 h-3.5"></i>
                                <span>Run Code</span>
                            </button>
                            <button id="submit-code-btn" class="bg-green-600 hover:bg-green-500 text-white font-semibold px-5 py-1.5 rounded-lg text-xs transition flex items-center space-x-1.5">
                                <i data-lucide="cloud-upload" class="w-3.5 h-3.5"></i>
                                <span>Submit Code</span>
                            </button>
                        </div>
                    </div>
                </div>

            </div>
        </div>

    </main>

    <!-- Monaco Loader scripts -->
    <script>
        var require = { paths: { vs: 'https://cdnjs.cloudflare.com/ajax/libs/monaco-editor/0.39.0/min/vs' } };
    </script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/monaco-editor/0.39.0/min/vs/loader.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/monaco-editor/0.39.0/min/vs/editor/editor.main.nls.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/monaco-editor/0.39.0/min/vs/editor/editor.main.js"></script>

    <script>
        // Set API key context empty as requested
        const apiKey = ""; 

        // Initial Sample Problems Array
        const problems = [
            {
                id: "1",
                title: "Two Sum",
                difficulty: "Easy",
                category: "Arrays & Hashing",
                statement: `<p>Given an array of integers <code>nums</code> and an integer <code>target</code>, return indices of the two numbers such that they add up to <code>target</code>.</p>
                <p>You may assume that each input would have <b>exactly one solution</b>, and you may not use the same element twice.</p>
                <p>You can return the answer in any order.</p>`,
                constraints: [
                    "2 <= nums.length <= 10^4",
                    "-10^9 <= nums[i] <= 10^9",
                    "-10^9 <= target <= 10^9",
                    "Only one valid answer exists."
                ],
                examples: [
                    { input: "nums = [2,7,11,15], target = 9", output: "[0,1]", explanation: "Because nums[0] + nums[1] == 9, we return [0, 1]." },
                    { input: "nums = [3,2,4], target = 6", output: "[1,2]" }
                ],
                starterTemplates: {
                    python: "class Solution:\n    def twoSum(self, nums: List[int], target: int) -> List[int]:\n        # Write your code here\n        pass",
                    javascript: "/**\n * @param {number[]} nums\n * @param {number} target\n * @return {number[]}\n */\nvar twoSum = function(nums, target) {\n    // Write your code here\n    \n};",
                    cpp: "class Solution {\npublic:\n    vector<int> twoSum(vector<int>& nums, int target) {\n        // Write your code here\n        \n    }\n};",
                    java: "class Solution {\n    public int[] twoSum(int[] nums, int target) {\n        // Write your code here\n        return new int[]{};\n    }\n}"
                }
            },
            {
                id: "2",
                title: "Reverse Integer",
                difficulty: "Medium",
                category: "Math",
                statement: `<p>Given a signed 32-bit integer <code>x</code>, return <code>x</code> with its digits reversed. If reversing <code>x</code> causes the value to go outside the signed 32-bit integer range <code>[-2^31, 2^31 - 1]</code>, then return <code>0</code>.</p>`,
                constraints: [
                    "-2^31 <= x <= 2^31 - 1"
                ],
                examples: [
                    { input: "x = 123", output: "321" },
                    { input: "x = -123", output: "-321" },
                    { input: "x = 120", output: "21" }
                ],
                starterTemplates: {
                    python: "class Solution:\n    def reverse(self, x: int) -> int:\n        # Write your code here\n        pass",
                    javascript: "/**\n * @param {number} x\n * @return {number}\n */\nvar reverse = function(x) {\n    // Write your code here\n    \n};",
                    cpp: "class Solution {\npublic:\n    int reverse(int x) {\n        // Write your code here\n        \n    }\n};",
                    java: "class Solution {\n    public int reverse(int x) {\n        // Write your code here\n        return 0;\n    }\n}"
                }
            },
            {
                id: "3",
                title: "Valid Parentheses",
                difficulty: "Easy",
                category: "Stack",
                statement: `<p>Given a string <code>s</code> containing just the characters <code>'('</code>, <code>')'</code>, <code>'{'</code>, <code>'}'</code>, <code>'['</code> and <code>']'</code>, determine if the input string is valid.</p>
                <p>An input string is valid if:</p>
                <ol class="list-decimal pl-5 space-y-1 mt-2">
                    <li>Open brackets must be closed by the same type of brackets.</li>
                    <li>Open brackets must be closed in the correct order.</li>
                    <li>Every close bracket has a corresponding open bracket of the same type.</li>
                </ol>`,
                constraints: [
                    "1 <= s.length <= 10^4",
                    "s consists of parentheses only '()[]{}'."
                ],
                examples: [
                    { input: 's = "()"', output: "true" },
                    { input: 's = "()[]{}"', output: "true" },
                    { input: 's = "(]"', output: "false" }
                ],
                starterTemplates: {
                    python: "class Solution:\n    def isValid(self, s: str) -> bool:\n        # Write your code here\n        pass",
                    javascript: "/**\n * @param {string} s\n * @return {boolean}\n */\nvar isValid = function(s) {\n    // Write your code here\n    \n};",
                    cpp: "class Solution {\npublic:\n    bool isValid(string s) {\n        // Write your code here\n        \n    }\n};",
                    java: "class Solution {\n    pu
