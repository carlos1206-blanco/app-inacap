// --- Lógica del Test Ishihara (Simulada) ---

// 1. Array de placas del test (debes tener las imágenes alojadas en una carpeta 'ishihara/' por ejemplo)
const ishiharaPlates = [
    { id: 1, image: 'ishihara/plate_1.jpg', correct: 12, notes: 'Placa de prueba: 12' },
    { id: 2, image: 'ishihara/plate_2.jpg', correct: 8, notes: 'Personas normales ven 8, daltónicos R/V ven 3' },
    { id: 3, image: 'ishihara/plate_3.jpg', correct: 29, notes: 'Personas normales ven 29, daltónicos R/V ven 70' }
    // Agrega más placas aquí...
];

let currentPlateIndex = 0;
const testCanvas = document.getElementById("testCanvas");
const testCtx = testCanvas.getContext("2d");
const testResultDiv = document.getElementById("testResult");


/**
 * Inicia o continúa el Test Ishihara
 */
function startTest() {
    currentPlateIndex = 0;
    document.getElementById('testArea').style.display = 'block';
    loadPlate(currentPlateIndex);
    toggleMenu(); // Cerrar menú al iniciar el test
}

/**
 * Carga una placa específica en el canvas
 */
function loadPlate(index) {
    if (index >= ishiharaPlates.length) {
        testResultDiv.textContent = "¡Test Terminado! Gracias por participar.";
        document.getElementById('testArea').style.display = 'none';
        return;
    }

    const plate = ishiharaPlates[index];
    testResultDiv.textContent = `Placa ${plate.id} de ${ishiharaPlates.length}. ¿Qué número ves?`;
    document.getElementById('testAnswer').value = ''; // Limpiar respuesta anterior
    
    // Cargar la imagen
    const plateImg = new Image();
    plateImg.onload = () => {
        // Asegurar que el canvas tenga un tamaño manejable
        const size = 300; 
        testCanvas.width = size;
        testCanvas.height = size;

        // Dibujar la imagen de la placa
        testCtx.drawImage(plateImg, 0, 0, size, size);

        // Opcional: Aplicar el filtro actual al canvas del test (Simula cómo vería el usuario con el filtro)
        if (selectedMatrix) {
            applyMatrix(testCanvas, testCtx);
        }
    };
    
    // **NOTA CLAVE:** Aquí es donde se necesita la ruta real a tu imagen.
    plateImg.src = plate.image; 
}


/**
 * Procesa la respuesta del usuario y avanza a la siguiente placa
 */
function submitAnswer() {
    const answerInput = document.getElementById('testAnswer');
    const answer = parseInt(answerInput.value);

    if (isNaN(answer)) {
        testResultDiv.textContent = `Por favor, ingresa un número.`;
        return;
    }
    
    // Aquí puedes implementar una lógica de verificación si lo deseas
    const currentPlate = ishiharaPlates[currentPlateIndex];
    
    // Lógica simple de verificación (puedes mejorarla)
    if (answer === currentPlate.correct) {
        testResultDiv.textContent = `¡Respuesta Correcta! (${currentPlate.correct}). Pasando a la siguiente placa.`;
    } else {
        testResultDiv.textContent = `Respuesta enviada: ${answer}. (La respuesta esperada es: ${currentPlate.correct}). Pasando a la siguiente.`;
    }
    
    // Avanzar a la siguiente placa después de un breve retraso
    setTimeout(() => {
        currentPlateIndex++;
        loadPlate(currentPlateIndex);
    }, 1500); // 1.5 segundos de pausa
}

// Inicializar el indicador de estado al cargar
document.addEventListener('DOMContentLoaded', () => {
    updateStatusIndicator(null);
});
