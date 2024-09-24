---
{"dg-publish":true,"permalink":"/class-and-function/"}
---



Function:

```python
from prettytable import PrettyTable  
  
persons = [  
	{'name': 'Marcus Aurelius', 'city': 'Rome', 'born': 121},  
	{'name': 'Victor Glushkov', 'city': 'Rostov on Don', 'born': 1923},  
	{'name': 'Ibn Arabi', 'city': 'Murcia', 'born': 1165},  
	{'name': 'Mao Zedong', 'city': 'Shaoshan', 'born': 1893},  
	{'name': 'Rene Descartes', 'city': 'La Haye en Touraine', 'born': 1596},  
]  
  
  
def console_re(data):  
	table = PrettyTable()  
	  
	table.field_names = list(data[0].keys())  
	  
	for row in data:  
		table.add_row(list(row.values()))  
		print(table)  
  
  
def web_re(data):  
	data_keys = data[0].keys()  
	  
	def line(row):  
		return '<tr>' + ''.join(f'<td>{row[key]}</td>' for key in data_keys) + '</tr>'  
	  
	output = [  
		'<table><tr>',  
		''.join(f'<th>{key}</th>' for key in data_keys),  
		'</tr>',  
		''.join(line(row) for row in data),  
		'</table>'  
	]  
  
print(''.join(output))
  
def mkd_re(data):  
	data_keys = list(data[0].keys())  
	  
	def line(row):  
	return '|' + '|'.join(str(row[key]) for key in data_keys) + '|\n'  
	  
	output = [  
	'| ' + ' | '.join(data_keys) + ' |\n',  
	'|' + '|'.join(['---'] * len(data_keys)) + '|\n',  
	''.join(line(row) for row in data)  
	]  
	  
	print(''.join(output))
  
  
renderers = {'console': console_re, 'web': web_re, 'markdown': mkd_re}  
  
  
def context(renderer_name):  
	return renderers.get(renderer_name, lambda data: f"Unknown renderer: {data}")  
  
  
con = context('console')  
web = context('web')  
mkd = context('markdown')  
  
print("\nConsoleRenderer:")  
con(persons)  
  
print("\nMarkdownRenderer:")  
mkd(persons)  
  
print("\nWebRenderer:")  
web(persons)
```

Class:

```python
from prettytable import PrettyTable  
persons = [  
	{'name': 'Marcus Aurelius', 'city': 'Rome', 'born': 121},  
	{'name': 'Victor Glushkov', 'city': 'Rostov on Don', 'born': 1923},  
	{'name': 'Ibn Arabi', 'city': 'Murcia', 'born': 1165},  
	{'name': 'Mao Zedong', 'city': 'Shaoshan', 'born': 1893},  
	{'name': 'Rene Descartes', 'city': 'La Haye en Touraine', 'born': 1596},  
]  
  
  
class Renderer:  
	def render(self, data):  
		self.data = data  
		print("Not implemented")  
  
  
class ConsoleRenderer(Renderer):  
	def render(self, data):  
		table = PrettyTable()  
		  
		table.field_names = list(data[0].keys())  
		  
		for row in data:  
			table.add_row(list(row.values()))  
		  
		print(table)  
  
class WebRenderer(Renderer):  
	def render(self, data):  
		data_keys = data[0].keys()  
	  
	def line(row):  
		return '<tr>' + ''.join(f'<td>{row[key]}</td>' for key in data_keys) + '</tr>'  
	  
	output = [  
		'<table><tr>',  
		''.join(f'<th>{key}</th>' for key in data_keys),  
		'</tr>',  
		''.join(line(row) for row in data),  
		'</table>'  
	]  
	  
	print(''.join(output))  
  
  
class MarkdownRenderer(Renderer):  
	def render(self, data):  
		keys = data[0].keys()  
		header = '|' + '|'.join(keys) + '|\n'  
		separator = '|' + '|'.join(['---'] * len(keys)) + '|\n'  
		rows = ''.join('|' + '|'.join(str(row[key]) for key in keys) + '|\n' for row in data)  
		output = header + separator + rows  
		print(output)  
  
class Context:  
	def __init__(self, renderer):  
		self.renderer = renderer  
  
	def process(self, data):  
		return self.renderer.render(data)  
  
print('Abstract Strategy:')  
Context(Renderer()).process(persons)  
  
print('\nConsoleRenderer:')  
Context(ConsoleRenderer()).process(persons)  
  
print('\nWebRenderer:')  
Context(WebRenderer()).process(persons)  
  
print('\nMarkdownRenderer:')  
Context(MarkdownRenderer()).process(persons)
```