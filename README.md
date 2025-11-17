import pygame
import random

# Inicializar pygame
pygame.init()

# Colores
BLANCO = (255, 255, 255)
NEGRO = (0, 0, 0)
VERDE = (0, 255, 0)
ROJO = (255, 0, 0)

# Tamaño de la pantalla
ANCHO = 600
ALTO = 400
pantalla = pygame.display.set_mode((ANCHO, ALTO))
pygame.display.set_caption("Snake Game")

# Reloj
clock = pygame.time.Clock()

# Tamaño del bloque de la serpiente
TAM_BLOQUE = 20
VELOCIDAD = 12

# Fuente
fuente = pygame.font.SysFont("Arial", 25)

def mostrar_mensaje(msg, color):
    texto = fuente.render(msg, True, color)
    pantalla.blit(texto, [ANCHO / 6, ALTO / 3])

def juego():
    game_over = False
    game_close = False

    x = ANCHO / 2
    y = ALTO / 2

    x_cambio = 0
    y_cambio = 0

    cuerpo_serpiente = []
    largo_serpiente = 1

    comida_x = round(random.randrange(0, ANCHO - TAM_BLOQUE) / TAM_BLOQUE) * TAM_BLOQUE
    comida_y = round(random.randrange(0, ALTO - TAM_BLOQUE) / TAM_BLOQUE) * TAM_BLOQUE

    while not game_over:

        while game_close:
            pantalla.fill(NEGRO)
            mostrar_mensaje("Perdiste! Pulsa C para continuar o Q para salir", ROJO)
            pygame.display.update()

            for event in pygame.event.get():
                if event.type == pygame.KEYDOWN:
                    if event.key == pygame.K_q:
                        game_over = True
                        game_close = False
                    if event.key == pygame.K_c:
                        juego()

        for event in pygame.event.get():
            if event.type == pygame.QUIT:
                game_over = True
            if event.type == pygame.KEYDOWN:
                if event.key == pygame.K_LEFT:
                    x_cambio = -TAM_BLOQUE
                    y_cambio = 0
                elif event.key == pygame.K_RIGHT:
                    x_cambio = TAM_BLOQUE
                    y_cambio = 0
                elif event.key == pygame.K_UP:
                    y_cambio = -TAM_BLOQUE
                    x_cambio = 0
                elif event.key == pygame.K_DOWN:
                    y_cambio = TAM_BLOQUE
                    x_cambio = 0

        # Si sale de pantalla → perder
        if x >= ANCHO or x < 0 or y >= ALTO or y < 0:
            game_close = True

        x += x_cambio
        y += y_cambio

        pantalla.fill(NEGRO)
        pygame.draw.rect(pantalla, ROJO, [comida_x, comida_y, TAM_BLOQUE, TAM_BLOQUE])

        cabeza_serpiente = []
        cabeza_serpiente.append(x)
        cabeza_serpiente.append(y)
        cuerpo_serpiente.append(cabeza_serpiente)

        if len(cuerpo_serpiente) > largo_serpiente:
            del cuerpo_serpiente[0]

        # Colisión consigo misma
        for segmento in cuerpo_serpiente[:-1]:
            if segmento == cabeza_serpiente:
                game_close = True

        for segmento in cuerpo_serpiente:
            pygame.draw.rect(pantalla, VERDE, [segmento[0], segmento[1], TAM_BLOQUE, TAM_BLOQUE])

        pygame.display.update()

        # Comida
        if x == comida_x and y == comida_y:
            comida_x = round(random.randrange(0, ANCHO - TAM_BLOQUE) / TAM_BLOQUE) * TAM_BLOQUE
            comida_y = round(random.randrange(0, ALTO - TAM_BLOQUE) / TAM_BLOQUE) * TAM_BLOQUE
            largo_serpiente += 1

        clock.tick(VELOCIDAD)

    pygame.quit()
    quit()

# Ejecutar el juego
juego()
