<template>
  <div class="vue-terminal" @click="handleFocus">
    <div class="terminal-header" v-if="showHeader">
      <h4>{{ title }}</h4>
      <ul class="shell-dots">
        <li class="shell-dots-red"></li>
        <li class="shell-dots-yellow"></li>
        <li class="shell-dots-green"></li>
      </ul>
    </div>

    <div>
      <div class="terminal-window" ref="terminalWindow">
        <p v-if="greeting">{{ greeting }}</p>
        <p v-if="defaultTaskCommandd">
          <span class="prompt"></span
          ><span class="cmd">{{ defaultTaskCommandd }}</span>
        </p>

        <div v-for="(item, index) in messageList" :key="index">
          <span>{{ item.time }}</span>
          <span v-if="item.label" :class="item.type"> {{ item.label }}</span>
          <pre class="cmd" v-if="!item.message.list"> {{ item.message }}</pre>
          <span class="cmd" v-else>
            <pre>{{ item.message.text }}</pre>
            <ul>
              <li v-for="(li, idx) in item.message.list" :key="idx">
                <span v-if="li.label" :class="li.type">{{ li.label }}:</span>
                <pre>{{ li.message }}</pre>
              </li>
            </ul>
          </span>
        </div>

        <p v-if="actionResult">
          <span class="cmd">{{ actionResult }}</span>
        </p>

        <p class="terminal-last-line" ref="terminalLastLine">
          
          <span>
            <span v-if="1 === 1" class="prompt">{{ prompt }}</span>
            <span class="prompt" v-else>\{{ title }}</span>
          </span>
          <span>{{ inputCommand }}</span>
          <span :class="lastLineClass" v-html="lastLineContent"></span>
          <input
            v-model="inputCommand"
            :disabled="1 === 12 && lastLineContent !== '&nbsp'"
            :autofocus="autoFocus"
            type="text"
            @keyup="handleCommand($event)"
            ref="inputBox"
            class="input-box"
          />
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted, nextTick } from "vue";

const commandList = {
  version: {
    description: "Return this project version",
    messages: [{ message: "1.0.0" }],
  },

  skills: {
    description: "List core technical proficiencies",
    messages: [
      { message: "Languages & Frameworks: C#, Go, Vue.js, .NET Core" },
      {
        message:
          "Infrastructure & Tools: Docker, Jenkins, Octopus Deploy, RabbitMQ, Azure",
      },
    ],
  },
  stack: {
    description: "Display current development environment details",
    messages: [
      { message: "OS: Linux / macOS" },
      { message: "Shell: Bash / Zsh with custom configuration" },
      { message: "Editor: Modern IDE with dark mode enabled" },
    ],
  },
  quote: {
    description: "Output a random software engineering maxim",
    messages: [
      {
        message:
          '"Simple things should be simple, complex things should be possible." — Alan Kay',
      },
    ],
  },
  socials: {
    description: "Show online handles and profiles",
    messages: [
      { message: "X (Twitter): @thewongandonly" },
      { message: "Website: https://thewongandonly.com" },
      { message: "Email: send.to.wong@gmail.com" },
    ],
  },
  uptime: {
    description: "Show system uptime",
    messages: [
      {
        message:
          "10:19:00 up 42 days, 13:37, 1 user, load average: 0.08, 0.03, 0.01",
      },
    ],
  },
  uname: {
    description: "Print system information",
    messages: [
      { message: "Linux terminal-node 6.1.0-generic x86_64 GNU/Linux" },
    ],
  },
  env: {
    description: "Print environment variables",
    messages: [
      { message: "SHELL=/bin/zsh" },
      { message: "THEME=dark" },
      { message: "EDITOR=nvim" },
      { message: "LANG=en_US.UTF-8" },
    ],
  },
  history: {
    description: "List recent session history",
    messages: [
      { message: "1  git status" },
      { message: "2  docker compose up -d" },
      { message: "3  go run main.go" },
      { message: "4  npm run dev" },
    ],
  },

  "git help": {
    description: "git help",
    messages: [
      {
        message: `usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
          [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
          [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
          [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
          <command> [<args>]

          These are common Git commands used in various situations:

          start a working area (see also: git help tutorial)
            clone      Clone a repository into a new directory
            init       Create an empty Git repository or reinitialize an existing one

          work on the current change (see also: git help everyday)
            add        Add file contents to the index
            mv         Move or rename a file, a directory, or a symlink
            reset      Reset current HEAD to the specified state
            rm         Remove files from the working tree and from the index

          examine the history and state (see also: git help revisions)
            bisect     Use binary search to find the commit that introduced a bug
            grep       Print lines matching a pattern
            log        Show commit logs
            show       Show various types of objects
            status     Show the working tree status

          grow, mark and tweak your common history
            branch     List, create, or delete branches
            checkout    Switch branches or restore working tree files
            commit     Record changes to the repository
            diff       Show changes between commits, commit and working tree, etc
            merge      Join two or more development histories together
            rebase     Reapply commits on top of another base tip
            tag        Create, list, delete or verify a tag object signed with GPG

          collaborate (see also: git help workflows)
            fetch      Download objects and refs from another repository
            pull       Fetch from and integrate with another repository or a local branch
            push       Update remote refs along with associated objects

          'git help -a' and 'git help -g' list available subcommands and some
          concept guides. See 'git help <command>' or 'git help <concept>'
          to read about a specific subcommand or concept.
          `,
      },
    ],
  },
};

// const USER_ID = parseInt(Math.random() * 1000)
function generateTime() {
  const timeNow = new Date();
  const hours = timeNow.getHours();
  const minutes = timeNow.getMinutes();
  const seconds = timeNow.getSeconds();
  let timeString = "" + hours;
  timeString += (minutes < 10 ? ":0" : ":") + minutes;
  timeString += (seconds < 10 ? ":0" : ":") + seconds;
  return timeString;
}

const mockData = [
  {
    time: generateTime(),
    type: "system",
    label: "System",
    message:
      "Welcome to FakeTerminal, this is an example to show you what this project can do.",
  },
  {
    time: generateTime(),
    type: "info",
    label: "Info",
    message: "Terminal Initializing ............",
  },
  {
    time: generateTime(),
    type: "warning",
    label: "warning",
    message: "This is a warning message!",
  },
  {
    time: generateTime(),
    type: "error",
    label: "Error",
    message: "Oops, something went wrong!",
  },
  {
    time: generateTime(),
    type: "success",
    label: "Success",
    message: "Take it easy! everything OK!",
  },
];

const taskList = {
  echo: {
    description: "Echoes input",
    echo(pushToList, input) {
      input = input.split(" ");
      input.splice(0, 1);
      const p = new Promise((resolve) => {
        pushToList({
          time: generateTime(),
          label: "Echo",
          type: "success",
          message: input.join(" "),
        });
        resolve({ type: "success", label: "", message: "" });
      });
      return p;
    },
  },
  defaultTask: {
    description:
      "This is a default task aimed to show you the power of this project.",
    defaultTask(pushToList) {
      let i = 0;
      const p = new Promise((resolve) => {
        const interval = setInterval(() => {
          mockData[i].time = generateTime();
          pushToList(mockData[i]);
          i++;
          if (!mockData[i]) {
            clearInterval(interval);
            resolve({
              type: "success",
              label: "Success",
              message: "Example Over!",
            });
          }
        }, 500);
      });
      return p;
    },
  },
  open: {
    description: "Open a specified url in a new tab.",
    open(pushToList, input) {
      const p = new Promise((resolve, reject) => {
        let url = input.split(" ")[1];
        if (!url) {
          reject({
            type: "error",
            label: "Error",
            message: "a url is required!",
          });
          return;
        }
        pushToList({ type: "success", label: "Success", message: "Opening" });

        if (input.split(" ")[1].indexOf("http") === -1) {
          url = "http://" + input.split(" ")[1];
        }
        window.open(url, "_blank");
        resolve({ type: "success", label: "Done", message: "Page Opened!" });
      });
      return p;
    },
  },
};

const props = defineProps({
  commandList: {
    required: false,
    default: () => ({}),
  },
  taskList: {
    required: false,
    default: () => ({}),
  },
  title: {
    required: false,
    type: String,
    default: "FakeTerminal",
  },
  showHeader: {
    required: false,
    type: Boolean,
    default: true,
  },
  greeting: {
    required: false,
    type: String,
    default: undefined,
  },
  defaultTaskCommandd: {
    required: false,
    type: String,
    default: "inspired by https://github.com/dongsuo/vue-terminal",
  },
  defaultTask: {
    required: false,
    type: String,
    default: "version",
  },
  prompt: {
    required: false,
    default: "$ ->",
  },
  showHelpMessage: {
    required: false,
    default: true,
  },
  unknownCommandMessage: {
    required: false,
    default: undefined,
  },
  autoFocus: {
    required: false,
    default: true,
  },
});

// Reactive state
const messageList = ref([]);
const actionResult = ref("");
const lastLineContent = ref("...");
const inputCommand = ref("");
const historyIndex = ref(0);
const commandHistory = ref([]);

// Template refs
const terminalWindow = ref(null);
const terminalLastLine = ref(null);
const inputBox = ref(null);

// Computed
const lastLineClass = computed(() => {
  if (lastLineContent.value === "&nbsp") {
    return "cursor";
  } else if (lastLineContent.value === "...") {
    return "loading";
  }
});

const supportingCommandList = Object.keys(
  props.commandList.length > 0 ? props.commandList : commandList,
).concat(Object.keys(props.taskList.length > 0 ? props.taskList : taskList));

// Methods
const handleFocus = () => {
  if (inputBox.value) {
    inputBox.value.focus();
  }
};

const pushToList = (message) => {
  messageList.value.push(message);
  autoScroll();
};

const autoScroll = () => {
  nextTick(() => {
    if (terminalWindow.value && terminalLastLine.value) {
      terminalWindow.value.scrollTop = terminalLastLine.value.offsetTop;
    }
  });
};

const handlekeyEvent = (e) => {
  switch (e.keyCode) {
    case 38:
      historyIndex.value =
        historyIndex.value === 0 ? 0 : historyIndex.value - 1;
      inputCommand.value = commandHistory.value[historyIndex.value] || "";
      break;
    case 40:
      historyIndex.value =
        historyIndex.value === commandHistory.value.length
          ? commandHistory.value.length
          : historyIndex.value + 1;
      inputCommand.value = commandHistory.value[historyIndex.value] || "";
      break;
    default:
      break;
  }
};

const printHelp = (input) => {
  const cl = props.commandList.length > 0 ? props.commandList : commandList;
  const tl = props.taskList.length > 0 ? props.taskList : taskList;

  if (!input) {
    pushToList({ message: "Here is a list of supporting commands:" });

    supportingCommandList
      .filter((c) => c !== "defaultTask")
      .forEach((command) => {
        if (cl[command]) {
          pushToList({
            type: "success",
            label: command,
            message: "---> " + cl[command].description,
          });
        } else {
          pushToList({
            type: "success",
            label: command,
            message: "---> " + tl[command].description,
          });
        }
      });
    pushToList({
      message: "Enter help <command> to get help for a particular command.",
    });
  } else {
    const command = cl[input] || tl[input];
    if (command) {
      pushToList({ message: command.description });
    }
  }
  autoScroll();
};

const handleRun = (taskName, input) => {
  const tl = props.taskList.length > 0 ? props.taskList : taskList;

  if (!tl[taskName] || !tl[taskName][taskName]) return Promise.resolve();
  lastLineContent.value = "...";
  return tl[taskName][taskName](pushToList, input)
    .then((done) => {
      pushToList(done);
      lastLineContent.value = "&nbsp";
    })
    .catch((error) => {
      pushToList(
        error || {
          type: "error",
          label: "Error",
          message: "Something went wrong!",
        },
      );
      lastLineContent.value = "&nbsp";
    });
};

const handleCommand = (e) => {
  if (e.keyCode !== 13) {
    handlekeyEvent(e);
    return;
  }
  commandHistory.value.push(inputCommand.value);
  historyIndex.value = commandHistory.value.length;

  const tl = props.taskList.length > 0 ? props.taskList : taskList;
  const cl = props.commandList.length > 0 ? props.commandList : commandList;

  if (typeof props.prompt !== "undefined") {
    pushToList({ message: `${props.prompt} ${inputCommand.value} ` });
  } else {
    pushToList({ message: `$ \\${props.title} ${inputCommand.value} ` });
  }
  if (!inputCommand.value) return;

  const commandArr = inputCommand.value.split(" ");
  if (commandArr[0] === "help") {
    printHelp(commandArr[1]);
  } else if (cl[inputCommand.value]) {
    cl[inputCommand.value].messages.forEach((item) => pushToList(item));
  } else if (tl[commandArr[0]]) {
    handleRun(commandArr[0], inputCommand.value);
  } else {
    if (props.unknownCommandMessage) {
      pushToList(props.unknownCommandMessage);
    } else {
      pushToList({ level: "System", message: "Unknown Command." });
      pushToList({
        level: "System",
        message: 'type "help" to get a supporting command list.',
      });
    }
  }
  inputCommand.value = "";
  autoScroll();
};

// Lifecycle hooks
onMounted(async () => {
  if (props.defaultTask) {
    await handleRun(props.defaultTask);
  }
  if (props.showHelpMessage) {
    pushToList({
      level: "System",
      message: 'Type "help" to get a supporting command list.',
    });
  }
  lastLineContent.value = "&nbsp";
  if (props.autoFocus) {
    handleFocus();
  }
});
</script>
<style scoped>
.vue-terminal {
  position: relative;
  width: 100%;
  border-radius: 6px;
  color: #00ff66;
  margin-bottom: 10px;
  max-height: 580px;
  top: 200px;
  box-shadow: 0 0 20px rgba(0, 255, 102, 0.2);
  border: 1px solid #00ff66;
}

.vue-terminal .terminal-window {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  overflow: auto;
  z-index: 1;
  margin-top: 30px;
  max-height: 500px;
  padding-top: 50px;
  background-color: #050d08;
  min-height: 140px;
  padding: 20px;
  font-weight: normal;
  font-family: Monaco, Menlo, Consolas, monospace;
  color: #00ff66;
}

.vue-terminal .terminal-window pre {
  font-family: Monaco, Menlo, Consolas, monospace;
  white-space: pre-wrap;
  display: inline;
}

.vue-terminal .terminal-window p {
  overflow-wrap: break-word;
  word-break: break-all;
  font-size: 13px;
}

.vue-terminal .terminal-window p .cmd {
  line-height: 24px;
}

/* Matrix Green Badge Styles */
.vue-terminal .terminal-window p .info {
  padding: 2px 4px;
  background: #003311;
  color: #00ff66;
  border: 1px solid #00ff66;
}

.vue-terminal .terminal-window p .warning {
  padding: 2px 4px;
  background: #332200;
  color: #ffcc00;
  border: 1px solid #ffcc00;
}

.vue-terminal .terminal-window p .success {
  padding: 2px 4px;
  background: #002200;
  color: #00ff66;
  border: 1px solid #00ff66;
}

.vue-terminal .terminal-window p .error {
  padding: 2px 4px;
  background: #330000;
  color: #ff3333;
  border: 1px solid #ff3333;
}

.vue-terminal .terminal-window p .system {
  padding: 2px 4px;
  background: #112211;
  color: #88ffaa;
  border: 1px solid #88ffaa;
}

.terminal-header {
  position: absolute;
  z-index: 2;
  top: 0;
  right: 0;
  left: 0;
  background-color: #0a1f12;
  text-align: center;
  padding: 4px;
  border-top-left-radius: 5px;
  border-top-right-radius: 5px;
  border-bottom: 1px solid #00ff66;
}

.terminal-header h4 {
  font-size: 14px;
  margin: 5px;
  letter-spacing: 1px;
  color: #00ff66;
}

.terminal-header ul.shell-dots {
  position: absolute;
  top: 7px;
  left: 8px;
  padding-left: 0;
  margin: 0;
}

.terminal-header ul.shell-dots li {
  display: inline-block;
  width: 10px;
  height: 10px;
  border-radius: 5px;
  margin-left: 6px;
}

.terminal-header ul .shell-dots-red {
  background-color: #ff3333;
}

.terminal-header ul .shell-dots-yellow {
  background-color: #ffcc00;
}

.terminal-header ul .shell-dots-green {
  background-color: #00ff66;
}

.vue-terminal .terminal-window .prompt::before {
  content: "";
  margin-right: 0px;
}

.vue-terminal .terminal-window .cursor {
  margin: 0;
  background-color: #00ff66;
  animation: blink 1s step-end infinite;
  -webkit-animation: blink 1s step-end infinite;
  margin-left: -5px;
}

@keyframes blink {
  50% {
    visibility: hidden;
  }
}

@-webkit-keyframes blink {
  50% {
    visibility: hidden;
  }
}

.vue-terminal .terminal-window .loading {
  display: inline-block;
  width: 0;
  overflow: hidden;
  overflow-wrap: normal;
  animation: load 1.2s step-end infinite;
  -webkit-animation: load 1.2s step-end infinite;
}

@keyframes load {
  0% {
    width: 0px;
  }
  20% {
    width: 5px;
  }
  40% {
    width: 10px;
  }
  60% {
    width: 15px;
  }
  80% {
    width: 20px;
  }
}

@-webkit-keyframes load {
  0% {
    width: 0px;
  }
  20% {
    width: 5px;
  }
  40% {
    width: 10px;
  }
  60% {
    width: 15px;
  }
  80% {
    width: 20px;
  }
}

.terminal-last-line {
  font-size: 0;
  word-spacing: 0;
  letter-spacing: 0;
  color: #00ff66;
}

.input-box {
  position: relative;
  background: #050d08;
  border: none;
  width: 1px;
  opacity: 0;
  cursor: default;
}

.input-box:focus {
  outline: none;
  border: none;
}
</style>
