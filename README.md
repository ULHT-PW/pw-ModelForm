# Criação de uma classe do tipo ModelForm

Considere que temos uma classe Tarefa com um conjunto de atributos. 
```Python
from django.db import models

class Tarefa(models.Model):
    titulo = models.CharField(max_length=200)
    prioridade = models.IntegerField(default=1)
    concluido = models.BooleanField(default=False)
    criado = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.titulo
 ```

ModelForm permite criar um formulário associado à classe Tarefa, com especificações detalhadas do que deve ser mostrado no formulário. Em baixo está o código para criação de um formulário, com descrição dos seus elementos. 

```Python
from django import forms
from django.forms import ModelForm
from .models import Tarefa

class TarefaForm(ModelForm):
    class Meta:
        model = Tarefa   # especifica a classe de models.py à qual este formulário está associado
                
        # fields permite especificar os campos da classe que queremos que apareçam no formulário. 
        #   - '__all__' apresenta todos.
        #   - podemos ter um subset: fields = ['titulo', 'prioridade']
        # alternativamente, pode-se usar a variável exclude para especificar os campos que se pretendem excluir do formulário 
        fields = '__all__'
   
        # Para um conjunto de propriedade da classe (titulo, prioridade, concluido, etc), 
        # o dicionário widgets permite configurar o elemento HTML input correspondente, 
        # através de um dicionario de atributos de formatação (especificação de classes, placeholder, propriedades, etc).
        widgets = {
            'titulo': forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'descrição da tarefa...'}),
            'prioridade': forms.NumberInput(attrs={'class': 'form-control', 'max': 3, 'min': 1}),
        }

       # o dicionário labels especifica o texto a exibir junto à janela de inserção
        labels = {
            'titulo': 'Título',
            'concluido': 'Concluída',
        }


       # o dicionário help_texts contém, para um atributo, um texto auxiliar a apresentar por baixo da janela de inserção
        help_texts = {
            'prioridade': 'prioridade: baixa=1, media=2, alta=3',
        }
```
