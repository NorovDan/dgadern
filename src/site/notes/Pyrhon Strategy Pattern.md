---
{"dg-publish":true,"permalink":"/pyrhon-strategy-pattern/"}
---


from datetime import datetime  
  
items = [  
    {  
        'name': 'keyboard',  
        'price': '$30',  
        'date': '10.08.2024',  
    },  
    {  
        'name': 'клавиатура',  
        'price': 'Rub4000',  
        'date': '11.08.2024',  
    },  
    {  
        'name': 'Монитор',  
        'price': 'Rub40000',  
        'date': '11.07.2024',  
    },  
    {  
        'name': 'Монитор 2',  
        'price': '$400',  
        'date': '11.07.2024',  
    },  
]  
  
  
def price_conversion(value, rate):  
    if value.startswith('$'):  
        return int(value[1:]) * rate  
  
    return int(value[3:])  
  
  
def sorting_name(items, sorting_parameter, rate=None):  
    if sorting_parameter == 'name':  
        return sorted(items, key=lambda x: x[sorting_parameter].lower())  
  
def sorting_price(items, sorting_parameter, rate=None):  
    if sorting_parameter == 'price':  
        return sorted(items, key=lambda x: price_conversion(x[sorting_parameter], rate))  
  
def sorting_date(items, sorting_parameter, rate=None):  
    if sorting_parameter == 'date':  
        return sorted(items, key=lambda x: datetime.strptime(x[sorting_parameter], '%d.%m.%Y'))  
  
name_sorting = sorting_name(items, 'name')  
print('name', name_sorting)  
  
price_sorting = sorting_price(items, 'price', 90)  
print('price', price_sorting)  
  
date_sorting = sorting_date(items, 'date')  
print('date', date_sorting)