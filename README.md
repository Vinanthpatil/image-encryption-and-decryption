from PIL import Image
import random
def encrypt_image(image_path, swap_count=1000):
    image = Image.open(image_path)
    pixels = image.load()
    width, height = image.size
    for _ in range(swap_count):
        x1, y1 = random.randint(0, width-1), random.randint(0, height-1)
        x2, y2 = random.randint(0, width-1), random.randint(0, height-1)
        pixels[x1, y1], pixels[x2, y2] = pixels[x2, y2], pixels[x1, y1]
    for x in range(width):
        for y in range(height):
            r, g, b = pixels[x, y]
            pixels[x, y] = (r ^ 255, g ^ 255, b ^ 255)
    encrypted_image_path = 'encrypted_image.png'
    image.save(encrypted_image_path)
    print(f"Image encrypted and saved as {encrypted_image_path}")
    return encrypted_image_path
def decrypt_image(encrypted_image_path, swap_count=1000):
    image = Image.open(encrypted_image_path)
    pixels = image.load()
    width, height = image.size
    for x in range(width):
        for y in range(height):
            r, g, b = pixels[x, y]
            pixels[x, y] = (r ^ 255, g ^ 255, b ^ 255)
    random.seed(0) 
    swap_indices = []
    for _ in range(swap_count):
        x1, y1 = random.randint(0, width-1), random.randint(0, height-1)
        x2, y2 = random.randint(0, width-1), random.randint(0, height-1)
        swap_indices.append((x1, y1, x2, y2))
    for x1, y1, x2, y2 in reversed(swap_indices):
        pixels[x1, y1], pixels[x2, y2] = pixels[x2, y2], pixels[x1, y1]
    decrypted_image_path = 'decrypted_image.png'
    image.save(decrypted_image_path)
    print(f"Image decrypted and saved as {decrypted_image_path}")
if __name__ == '__main__':
    input_image_path = 'C:/Users/Vinanth patil/Downloads/WhatsApp Image 2024-09-28 at 9.32.28 AM.jpeg' 
    encrypted_image_path = encrypt_image(input_image_path)
    decrypt_image(encrypted_image_path)
