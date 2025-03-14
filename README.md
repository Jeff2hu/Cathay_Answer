# Cathay_Answer

## **1️ 排序函式**

### **Q1. 依照 `firstName + lastName + customerID` 進行排序**

```javascript
function sortUserName(users) {
  // 檢查是否為有效的陣列
  if (!users || !Array.isArray(users) || users.length <= 0) return false;

  return users.sort((a, b) => {
    const nameA = a.firstName + a.lastName + a.customerID;
    const nameB = b.firstName + b.lastName + b.customerID;

    // 使用 `localeCompare` 來確保語言排序正確，並啟用 `numeric`
    return nameA.localeCompare(nameB, undefined, { numeric: true });
  });
}
```

### **Q2. 依照 `profession` 權重排序**

```javascript
function sortByType(users) {
  // 先把題目的權重設成一個object
  const professionOrder = {
    systemAnalytics: 5,
    engineer: 4,
    productOwner: 3,
    freelancer: 2,
    student: 1,
  };

  // 找到相對應的權重做排序
  return users.sort((a, b) => {
    return professionOrder[b.profession] - professionOrder[a.profession];
  });
}
```

## **2 CSS 選擇器問題**

### **Q1 li.item 在 shop-list 內的優先級**

```
  跟li.item同層級理論上會以後者.item為主才對

  .container .shop-list:first-child .item {
    color: blue;
  }

```

### **Q2 選擇 li 的奇數行**

```
  .container .shop-list li:nth-child(odd) {
    background-color: #f0f0f0;
  }

```

## **3 取得陣列中的唯一數值**

```javascript
function getUniqueNumber(items) {
  const uniqueNumber = [...new Set(items)];
  console.log(uniqueNumber);
}
```

## **4 什麼是虛擬 DOM？**

Q: What is virtual DOM and what purpose does it aim to solve?

A: 虛擬 DOM 是一個輕量的 JavaScript 物件，它的主要用途是 避免直接操作真實 DOM，進而提升效能。 React 在狀態變更時，會產生新的虛擬 DOM，並透過 Diffing 演算法計算最小變更，僅更新需要變更的部分，避免不必要的重新渲染。

## **5. never 和 void 的區別**

Q: What is the difference between never and void in TypeScript?

A: never：表示 函式永遠不會回傳值，包含 拋出錯誤 或 無限迴圈 的情況。
void：表示 函式可以執行，但不會有明確的回傳值。

## **6. 框架型網站 vs. 傳統網站**

Q: What is the difference between a framework-based website and a normal website?

A: 框架提供了一套規範，讓開發和維護更有一致性。

例如，在 React 中，我們不需要手動 addEventListener，因為 React 的事件系統已經幫我們處理。
此外，React 的 虛擬 DOM 和 Diffing 機制 讓畫面更新更加高效。

## **7. 在 React 中如何記住不同使用者的任務**

```javascript
import { useState } from "react";

export default function TaskManager() {
  const [isPersonAlice, setIsPersonAlice] = useState(true);
  const [tasks, setTasks] = useState({ Alice: 0, Bob: 0 });

  const handleCompleteTask = () => {
    setTasks((prevTasks) => ({
      ...prevTasks,
      [isPersonAlice ? "Alice" : "Bob"]:
        prevTasks[isPersonAlice ? "Alice" : "Bob"] + 1,
    }));
  };

  return (
    <div>
      <TaskCounter
        name={isPersonAlice ? "Alice" : "Bob"}
        tasks={tasks[isPersonAlice ? "Alice" : "Bob"]}
        onCompleteTask={handleCompleteTask}
      />
      <button onClick={() => setIsPersonAlice(!isPersonAlice)}>
        Switch Person
      </button>
    </div>
  );
}

function TaskCounter({ name, tasks, onCompleteTask }) {
  return (
    <>
      <h1>
        {name}'s tasks: {tasks}
      </h1>
      <button onClick={onCompleteTask}>Complete Task</button>
    </>
  );
}
```

## **8. React: 找出錯誤並給予意見**

Q: 分析以下程式碼

A:

1.  todos 是 陣列，不能 const { id, text, studyPoint } = todos
2.  todo.completed = !todo.completed 直接修改 state，不會觸發 re-render
3.  setTodos(todos) state 沒變化，不會重新渲染
4.  basePoints(e.target.value) 應該要透過 setState
5.  value1 不清楚來源
6.  <button onClick={toggleTodo(id)}> 會直接執行函式，應改成 onClick={() => toggleTodo(id)}

## **9. 找出錯誤並給予意見**

Q: 如何改善這段程式碼？

A: 使用 Context API 來避免 Prop Drilling，減少層層傳遞 props 的問題。

## **10. 使用 useRef 控制 input 聚焦**

```javascript
function SearchButton({ inputRef }) {
  return <button onClick={() => inputRef.current.focus()}>Search</button>;
}

function SearchInput({ inputRef }) {
  return <input ref={inputRef} />;
}

export default function Page() {
  const inputRef = useRef(null);

  return (
    <>
      <nav>
        <SearchButton inputRef={inputRef} />
      </nav>
      <SearchInput inputRef={inputRef} />
    </>
  );
}
```

## **11. CodeSandBox 實作和改進**

URL: https://codesandbox.io/p/devbox/task-management-forked-rhfcgs?workspaceId=ws_7ooVWPBr5qbSHijQCyoPag

錯誤:

1. updateTask 直接修改 task 而非 prevTask，可能導致問題
2. deleteTask 也是直接操作 task 而非 prevTask
3. filterTask 已經在 TaskList 處理，不需要在 context 中額外定義
4. window.confirm() 是同步阻塞式，應該改用 Dialog（彈出視窗） 來詢問使用者
