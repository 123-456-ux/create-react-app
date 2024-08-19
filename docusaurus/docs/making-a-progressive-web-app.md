// Importamos las librerías necesarias
import { Pedometer } from 'expo-sensors';
import * as Notifications from 'expo-notifications';

// Creamos una función para medir el ritmo cardíaco
async function measureHeartRate() {
  // Iniciamos el pedometer (contador de pasos)
  const subscription = Pedometer.watchStepCount(result => {
    console.log('Current step count:', result.steps);
  
    // Si el paso por minuto es mayor a un umbral predeterminado (por ejemplo, 100 bpm)
    if (result.steps >= 120) {
      // Notificamos al usuario
      const notification = await Notifications.scheduleNotificationAsync({
        content: {
          title: 'Alerta de ritmo cardíaco',
          body: 'Tu ritmo cardíaco es alto. Por favor, descansa y busca ayuda si es necesario.',
        },
        trigger: { seconds: 60 }, // Notificamos cada minuto
      });
      
      // Llamamos a emergencias (por ejemplo, al número de emergencias en México: 911)
      await , {
        method: 'POST',
        headers: {
          Accept: 'application/json',
          'Content-Type': 'application/json',
        },
        body: JSON.stringify({
          emergencyNumber: '911',
        }),
      });
    }
  });
  
  // Detenemos la suscripción al pedometer cuando no sea necesario
  return () => subscription.remove();
}
