npx react-native init MeuApp
cd MeuApp 
npm install @react-navigation/native @react-navigation/bottom-tabs react-native-reanimated react-native-gesture-handler
// TreinoScreen.js
import React, { useState, useEffect } from 'react';
import { View, Text, Button } from 'react-native';

const TreinoScreen = () => {
  const [timer, setTimer] = useState(0);
  const [exercicioRealizado, setExercicioRealizado] = useState(false);

  useEffect(() => {
    const interval = setInterval(() => {
      if (timer < 120) {
        setTimer(timer + 1);
      }
    }, 1000);

    return () => clearInterval(interval);
  }, [timer]);

  const handleToggleExercicio = () => {
    setExercicioRealizado(!exercicioRealizado);
  };

  return (
    <View>
      <Text>{`Tempo: ${timer} segundos`}</Text>
      <Button title="Toggle Exercício" onPress={handleToggleExercicio} />
    </View>
  );
};

export default TreinoScreen;
// PagamentoScreen.js
import React, { useState } from 'react';
import { View, Text, Button } from 'react-native';

const PagamentoScreen = () => {
  const [pagamentos, setPagamentos] = useState(Array(12).fill(false));

  const handlePagar = (mes) => {
    const newPagamentos = [...pagamentos];
    newPagamentos[mes] = !newPagamentos[mes];
    setPagamentos(newPagamentos);
  };

  return (
    <View>
      {pagamentos.map((pago, index) => (
        <View key={index} style={{ flexDirection: 'row', alignItems: 'center' }}>
          <Text>{`Mês ${index + 1}: `}</Text>
          <Text style={{ color: pago ? 'green' : 'black' }}>{pago ? 'Pago' : 'Não Pago'}</Text>
          <Button title="Pagar" onPress={() => handlePagar(index)} />
        </View>
      ))}
    </View>
  );
};

export default PagamentoScreen;
// App.js
import React from 'react';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import TreinoScreen from './screens/TreinoScreen';
import PagamentoScreen from './screens/PagamentoScreen';

const Stack = createStackNavigator();

const App = () => {
  return (
    <NavigationContainer>
      <Stack.Navigator initialRouteName="Treino">
        <Stack.Screen name="Treino" component={TreinoScreen} />
        <Stack.Screen name="Pagamento" component={PagamentoScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
};

export default App;
npx react-native run-android  # ou npx react-native run-ios
