---
{"dg-publish":true,"permalink":"/adapter/"}
---

# Case

Реализация паттерна "Адаптер" для система управления проектами, которая использует собственный формат для хранения данных о задачах, в которой есть
модуль для интеграции с внешним сервисом, который предоставляет информацию о задачах в формате JSON

```python
import json

class Task:
    def get_title(self):
        pass

    def get_description(self):
        pass

    def get_due_date(self):
       pass

    import json

class JsonTaskAdapter(Task):
    def __init__(self, json_data):
        self.json_data = json.loads(json_data)

    def get_title(self):
        return self.json_data['title']

    def get_description(self):
       return self.json_data['description']

    def get_due_date(self):
        return self.json_data['due_date']

json_data = '{"title": "Finish report", "description": "Complete the annual report by end of week", "due_date": "2023-10-10"}'
task = JsonTaskAdapter(json_data)

print(task.get_title())  
print(task.get_description())  
print(task.get_due_date())  
```

## Вывод
```python
Finish report
Complete the annual report by end of week
2023-10-10
```

# From JS to Python

```python
class ArrayToQueueAdapter:
    def __init__(self):
        self.queue = []

    def enqueue(self, data):
        self.queue.append(data)

    def dequeue(self):
        if self.queue:
            return self.queue.pop(0)  
        return None  
    
    def count(self):
        return len(self.queue)



queue = ArrayToQueueAdapter()
queue.enqueue('uno')
queue.enqueue('due')
queue.enqueue('tre')

while queue.count > 0:
    print(queue.dequeue())

```

```python
def array_to_queue_adapter(): 
	array = [] 
	
	def enqueue(data): 
		array.append(data) 
		
	def dequeue(): 
		return array.pop() 
	
	def count(): 
		return len(array) 
	
	return {'enq': enqueue, 'deq': dequeue, 'cnt': count} 

queue = array_to_queue_adapter() 
queue['enq']('uno') 
queue['enq']('due') 
queue['enq']('tre') 

while queue['cnt'](): 
	print(queue['deq']())
```

# Promisify

```python
import asyncio
from concurrent.futures import ThreadPoolExecutor

def read_file(file_name):
    with open(file_name, 'r') as f:
        return f.read()

async def promisify(func, *args):
    loop = asyncio.get_event_loop()
    with ThreadPoolExecutor() as pool:
        result = await loop.run_in_executor(pool, func, *args)
    return result

async def main():
    file_name = '1-promisify.py'  
    data = await promisify(read_file, file_name)
    print(f'File "{file_name}" size: {len(data)}')

if __name__ == '__main__':
    asyncio.run(main())

```

