import PIL.Image
import os.path
import time


def off_program(data_):
    if data_ == "exit":
        print("The program has been stopped! ")
        time.sleep(10)
        return quit()


print("If you want to stop the program, you can enter the text: exit")

img_flag = True
path = input("Enter the name of the png file you want to convert: \n")
off_program(path)

while '.jpg' not in path or os.path.exists(path) == False:
    print("Incorrect data entry! ")
    path = input("Enter the path to the image field again! \n")
    off_program(path)

img = PIL.Image.open(path)

width, height = img.size
aspect_ratio = height / width

while True:
    try:
        new_width = input("Enter a value from 100 to 1000: ")
        off_program(new_width)
        new_width = int(new_width)

        if new_width >= 100 and new_width <= 1000:
            pass
        else:
            print("Incorrect data entry! Please, repeat the input! ")
            continue

        break

    except ValueError:
        print("Incorrect data entry! Please, repeat the input! ")

new_height = aspect_ratio * new_width * 0.55
img = img.resize((new_width, int(new_height)))

img = img.convert('L')

chars = ["@", "-", "#", "%", "*", "/", "+", "&", "$", ",", ".", " "]

pixels = img.getdata()
new_pixels = [chars[pixel // 25] for pixel in pixels]
new_pixels = ''.join(new_pixels)
new_pixels_count = len(new_pixels)
ascii_image = [new_pixels[index:index + new_width] for index in range(0, new_pixels_count, new_width)]

ascii_image = "\n".join(ascii_image)
name_file = str(input("Enter the name of the file you want to save the image to\n"))
off_program(name_file)

with open(name_file + ".txt", "w") as f:
    f.write(ascii_image)

print(f"Your image has been saved to a file {name_file}.txt")

time.sleep(10)
