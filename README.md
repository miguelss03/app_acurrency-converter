# app_acurrency-converter


# index.js



import React, {Component} from 'react';
import { View, Text, StyleSheet, TextInput, TouchableOpacity, Keyboard  } from 'react-native';

import api from '../services/api'


class Conversor extends Component{

    constructor(props){
        super(props);
        this.state = {
            moedaA: props.moedaA,
            moedaB: props.moedaB,
            moedaB_valor: 0,
            valorConvertido: ""
        };

        this.converter = this.converter.bind(this);
    }

    async converter(){
        let de_para = this.state.moedaA + '_' + this.state.moedaB;
        const response = await api.get(`convert?q=${de_para}&compact=ultra&apiKey=9b007913dd6149a2cf4f`);
        let cotacao = response.data[de_para];
        
        let resultado = (cotacao * parseFloat(this.state.moedaB_valor));
        
        this.setState({
            valorConvertido: resultado.toFixed(2)
        });

        Keyboard.dismiss();
    }

    render(){
        const {moedaA, moedaB} = this.props;
        return(
            <View style={styles.container}>

                <Text style={styles.titulo}>{moedaA} para {moedaB}</Text>

                <Text style={styles.valorConvertido}>
                                {this.state.valorConvertido}         
                </Text>

                <TextInput
                placeholder="Valor a ser convertido"
                style={styles.areaInput}
                onChangeText={(moedaB_valor) => this.setState({ moedaB_valor})}
                keyboardType="numeric"
                />

                <TouchableOpacity style={styles.botaoArea} onPress={this.converter}>
                    <Text style={styles.botaoTexto}>Converter</Text>
                </TouchableOpacity>


                
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex:1,
        justifyContent: 'center',
        alignItems: 'center'
    },

    titulo:{
        fontSize: 35,
        fontWeight: 'bold',
        color: '#6959CD',           
    },

    areaInput:{
        width: 280,
        height:45,
        backgroundColor: '#C0C0C0',
        textAlign: 'center',
        marginTop: 15,        
        fontSize: 18,
        color: 'black',
        borderRadius: 8,
    },

    botaoArea: {
        width: 150,
        height: 45,
        backgroundColor: '#6A5ACD',
        borderRadius: 8,
        justifyContent: 'center',
        alignItems: 'center',
        marginTop:15,
       
    },

    botaoTexto:{
        fontSize: 18,
        fontWeight: 'bold',
        color: '#FFF'
    },

    valorConvertido:{
        fontSize: 28,
        fontWeight: 'bold',
        color: '#1C1C1C',
        marginTop: 15,
        backgroundColor: '#C0C0C0',
        width: 280,
        height: 45,
        textAlign: 'center',       
        borderRadius: 8
    }


});

export default Conversor;







# App.js

import React, {Component} from 'react';
import { 
    View, 
    Text,
    StyleSheet, 
} from 'react-native';


import Conversor from './src/Conversor';

class App extends Component {
    render(){
        return(
            <View style={styles.container}>

                
                
                <Conversor moedaA="EUR" moedaB="BRL" />

                
            </View>
        );
    }
}

const styles = StyleSheet.create({
    container: {
        flex:1,
        justifyContent: 'center',
        alignItems: 'center'
    },

});
export default App;









 # api.js
 
 
 import axios from 'axios';

////  BaseURL: https://free.currconv.com/api/v7/
////  =>   convert?q=USD_PHP&compact=ultra&apiKey=9b007913dd6149a2cf4f

const api = axios.create({
    baseURL: 'https://free.currconv.com/api/v7'
});

export default api;
 
 

