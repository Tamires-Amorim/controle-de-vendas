package com.example.controlevendas

import android.os.Bundle
import androidx.activity.ComponentActivity
import androidx.activity.compose.setContent
import androidx.compose.foundation.layout.*
import androidx.compose.foundation.lazy.LazyColumn
import androidx.compose.foundation.lazy.items
import androidx.compose.material.*
import androidx.compose.runtime.*
import androidx.compose.ui.Modifier
import androidx.compose.ui.unit.dp

data class Produto(val nome: String, val quantidade: Int, val preco: Double)

class MainActivity : ComponentActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContent {
            App()
        }
    }
}

@Composable
fun App() {
    var nome by remember { mutableStateOf("") }
    var quantidade by remember { mutableStateOf("") }
    var preco by remember { mutableStateOf("") }
    var produtos by remember { mutableStateOf(listOf<Produto>()) }

    Scaffold(topBar = {
        TopAppBar(title = { Text("Controle de Vendas") })
    }) {
        Column(modifier = Modifier.padding(16.dp)) {
            OutlinedTextField(
                value = nome,
                onValueChange = { nome = it },
                label = { Text("Nome do produto") },
                modifier = Modifier.fillMaxWidth()
            )
            Spacer(modifier = Modifier.height(8.dp))
            OutlinedTextField(
                value = quantidade,
                onValueChange = { quantidade = it },
                label = { Text("Quantidade") },
                modifier = Modifier.fillMaxWidth()
            )
            Spacer(modifier = Modifier.height(8.dp))
            OutlinedTextField(
                value = preco,
                onValueChange = { preco = it },
                label = { Text("Preço") },
                modifier = Modifier.fillMaxWidth()
            )
            Spacer(modifier = Modifier.height(8.dp))
            Button(onClick = {
                val qtd = quantidade.toIntOrNull() ?: 0
                val valor = preco.toDoubleOrNull() ?: 0.0
                if (nome.isNotBlank() && qtd > 0 && valor > 0) {
                    produtos = produtos + Produto(nome, qtd, valor)
                    nome = ""
                    quantidade = ""
                    preco = ""
                }
            }) {
                Text("Adicionar Produto") 
            }
            Spacer(modifier = Modifier.height(16.dp))
            Text("Estoque:", style = MaterialTheme.typography.h6)
            LazyColumn {
                items(produtos) { produto ->
                    Text("${produto.nome} - ${produto.quantidade} un - R$ ${produto.preco}")
                }
            }
        }
    }
}
