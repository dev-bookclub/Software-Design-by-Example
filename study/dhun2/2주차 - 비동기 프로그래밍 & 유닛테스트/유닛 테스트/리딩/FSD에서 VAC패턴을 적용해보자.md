
# 🚀 FSD(Feature-Sliced Design)에서 VAC 패턴을 UI에 적용했을 때의 장점

## 🔍 1. VAC 패턴이란?

**VAC (View-Action-Controller)** 패턴은 UI 컴포넌트의 역할을 명확히 구분하여 개발의 효율성과 유지보수성을 높이는 아키텍처입니다.

- **View (V):** 사용자에게 보여지는 UI (React/Vue 컴포넌트)
- **Action (A):** 사용자 입력 이벤트 처리 및 상태 변화 요청
- **Controller (C):** 비즈니스 로직 처리 및 상태 관리 (API 호출, 데이터 가공)

---

## 📦 2. FSD(FEATURE-SLICED DESIGN)와 VAC 패턴 결합

FSD는 기능 단위로 코드를 분리해 확장성과 유지보수성을 높이는 아키텍처입니다. 여기에 VAC 패턴을 적용하면 UI 컴포넌트의 역할이 명확해지고, 기능 단위로 관리하기 쉬워집니다.

### ✅ **폴더 구조 예시**

```
src/
├── entities/
│   └── task/
│       ├── model/          // 도메인 모델 및 상태 관리
├── features/
│   └── todo/
│       ├── ui/             // View (UI 컴포넌트)
│       │   ├── TodoList.tsx
│       │   └── TodoItem.tsx
│       ├── api/            // API 호출 관련 로직
│       │   └── todoApi.ts
│       ├── hooks/          // Custom Hooks (비즈니스 로직 처리)
│       │   └── useTodo.ts
│       └── model/          // 상태 관리
│           └── todoModel.ts
|
├── shared/
│   └── ui/
│       └── Button.tsx
└── pages/
    └── TodoPage.tsx
```

---


## 🎯 3. FSD + VAC 패턴의 장점

### ✅ 1) 역할 분리로 인한 코드 가독성 향상

- UI(View), 이벤트 처리(Action), **비즈니스 로직(Controller)**가 명확히 분리되어 코드 가독성이 좋아집니다.  
- 컴포넌트가 단순해져 **재사용성**과 **테스트**가 쉬워집니다.  

### ✅ 2) 유지보수성 및 확장성 강화

- 기능 단위로 폴더가 나뉘어 있어, 특정 기능만 수정하거나 추가하기 용이합니다.  
- 비즈니스 로직이 UI에서 분리되어 있어, 기능 변경 시 UI에 영향을 최소화합니다.  

### ✅ 3) 테스트 용이성

- **Controller**와 **Action**이 분리되어 있어, **단위 테스트(Unit Test)**와 **통합 테스트(Integration Test)**가 쉽습니다.  
- UI는 독립적으로 테스트할 수 있고, Action과 Controller는 비즈니스 로직을 중심으로 테스트할 수 있습니다.  

### ✅ 4) 효율적인 협업

- **디자이너**는 View(UI) 중심으로 작업하고, **개발자**는 Action과 Controller를 담당하여 협업이 효율적입니다.  
- 역할 분담이 명확해져 팀원 간의 충돌을 줄일 수 있습니다.  

### ✅ 5) 기능 추가 및 수정의 유연성

- 새로운 기능을 추가하거나 기존 기능을 수정할 때, 특정 역할에만 집중해 작업할 수 있습니다.  
- UI, Action, Controller가 독립적이기 때문에 확장성과 유연성이 뛰어납니다.  

---

## ✅ 4. 예시: TodoList 기능의 VAC 패턴 적용

### 📦 **1) View - UI 컴포넌트**

```javascript
import React, { useEffect, memo } from 'react';
import { useTodo } from '../hooks/useTodo';

const TodoList = () => {
  const { tasks, loadTasks } = useTodo();

  useEffect(() => {
    loadTasks();
  }, []);

  return (
    <div>
      <h1>할 일 목록</h1>
      <ul>
        {tasks.map(task => (
          <li key={task.id}>{task.content}</li>
        ))}
      </ul>
    </div>
  );
};

export default memo(TodoList);
```

### 📦 **2) Hook - 비즈니스 로직 처리**

```javascript
import { useState, useCallback } from 'react';
import { fetchTasks } from '../api/todoApi';

export const useTodo = () => {
  const [tasks, setTasks] = useState([]);

  const loadTasks = useCallback(async () => {
    const data = await fetchTasks();
    setTasks(data);
  }, []);

  return {
    tasks,
    loadTasks,
  };
};
```

### 📦 **3) API - 데이터 요청**

```javascript
import axios from 'axios';

export const fetchTasks = async () => {
  try {
    const response = await axios.get('/api/tasks');
    return response.data;
  } catch (error) {
    console.error('데이터를 불러오는 중 오류 발생:', error);
    return [];
  }
};
```

### 📦 **4) 테스트 - 단위 테스트 예시**

```javascript
import axios from 'axios';
import { fetchTasks } from '../api/todoApi';

jest.mock('axios');

test('할 일 목록을 불러온다.', async () => {
  axios.get.mockResolvedValue({ data: [{ id: 1, content: '테스트', completed: false }] });
  const tasks = await fetchTasks();
  expect(tasks).toEqual([{ id: 1, content: '테스트', completed: false }]);
});
```

---

## ✅ 5. 결론

- **VAC 패턴**과 **FSD**의 조합은 **UI**, **API**, **Hooks**의 역할 분리를 통해 **코드 가독성**, **유지보수성**, **확장성**을 극대화합니다.
- UI는 **순수한 컴포넌트**로 유지되고, 데이터 로직과 API 처리가 완벽히 분리되어 **효율적인 개발**과 **협업**이 가능해집니다. 🚀
