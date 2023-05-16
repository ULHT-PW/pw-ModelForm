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

ModelForm permite criar um formulário associado a uma modelo, com especificações detalhadas do que mostrar no formulário. Em baixo está o código para criação de um formulário

```Python
from django import forms
from django.forms import ModelForm
from .models import Tarefa

class TarefaForm(ModelForm):
    class Meta:
        model = Tarefa
        fields = '__all__'
        
        # widgets é um dicionário. A cada propriedade da classe (titulo, prioridade, concluido, etc), 
        # permite configurar o correspondente elemento input no formulário, um dicionario de atributos tais como propriedades ou classes,
        # para formatação de cada campo do formulário.
        widgets = {
            'titulo': forms.TextInput(attrs={'class': 'form-control', 'placeholder': 'descrição da tarefa...'}),
            'prioridade': forms.NumberInput(attrs={'class': 'form-control', 'max': 3, 'min': 1}),
        }


        # labels perrmite especificar o texto a exibir junto à janela de inserção
        labels = {
            'titulo': 'Título',
            'concluido': 'Concluída',
        }


        # help_texts são textos auxiliares apresentados por baixo de uma janela de inserção do formulário
        help_texts = {
            'prioridade': 'prioridade: baixa=1, media=2, alta=3',
        }
```
