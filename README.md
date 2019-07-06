# xtea3_lib
 Библиотека шифрования XTEA3 на С++.
 
 Иногда бывают ситуации, что нужен боле-менее надежный алгоритм, простой в реализации и быстрый.
 
 Можно использовать XOR, но этот алгоритм уязвим и расшифровать его вполне-себе можно, если будет необходимость.
 
 Есть достаточно популярный агоритм блочного симметричного шифрования XTEA и его модификации, в данном случае решил написать библиотеку модифицированного алгоритма XTEA3 (https://ru.wikipedia.org/wiki/XTEA).
 
 **Причина написания этой библиотеки:**
 
 1)Так и неувидел нормальной реализации на С++;
 
 2)Т.к. алгоритм блочный, то размер шифрованного образца, будет выровнен по длине блока, а длина блока у каждой реализации разная, поэтому в имеющихся реализаций, **программист сам должен это учитывать**, 
 
 **В МОЕЙ РЕАЛИЗАЦИИ ЭТО УЧИТЫВАТЬ НЕ НУЖНО**, при шифровки/расшифровки выделяется буфер уже выровненного размера и работа будет уже с этим буфером, также в зашифрованном буфере 
 
 хранится размер расшифрованного образца и размер расшифрованного образца.
 
 Все эти данные можно получить в методах библиотеки.
 
 3)В данной реализации можно шифровать любые данные, хоть строки, хоть бинарные данные.
 
 **Где можно использовать этот алгоритм:**

 1)Данный алгоритм с одной стороны простой в реализации и очень быстрый. 
 
 А с другой стороны, весьма надежный и тяжело определить, что данные пошифрованные, а не то-что это просто набор данных.
 
 Поэтому можно использовать в встраиваемых системах, где требуется защита данных.

 2)Можно использовать в протекторах и крипторах.
 
 Также отлично подойдет для защиты логов, например в кейлоггере и стилере.)

 3)Также можно использовать для шифровании больших объемов данных.

 ** Описание библиотеки: **

 1)Данный алгоритм использует 32-х байтный ключ, поэтому перед шифрованием нужно выделить под него память, например (uint32_t key[8] = {0x11, 0x22, 0x33, 0x44, 0x55, 0x66, 0x77, 0x88}).
 
 **Далее, всё просто: **
 
 2)Создаем класс:
 
 xtea3 *ptr_xtea_lib = new xtea3;
 
 3)У xtea3 есть следующие методы:
 
 ** - uint8_t *data_crypt(const uint8_t *data, const uint32_t key[8], uint32_t size); **
 
 Метод зашифрует данные, указателя uint8_t *data, ключём uint32_t key[8] и размером uint32_t size.
 
 Возвращает указатель на буфер пошифрованных данных (В случае ошибки, возратит NULL).
 
 ** - uint8_t *data_decrypt(const uint8_t *data, const uint32_t key[8], uint32_t size); **
 
 Метод расшифрует данные, указателя uint8_t *data, ключём uint32_t key[8] и размером uint32_t size.
 
 Возвращает указатель на буфер расшифрованных данных (В случае ошибки, возратит NULL).
 
 ** - uint32_t get_decrypt_size(void); **
 
 Метод вернет размер буфера зашифрованных данных.
 
 ** uint32_t get_crypt_size(void); **
 
 Метод вернет размер буфера расшифрованных данных
 
 ** void free_ptr(uint8_t *ptr); **
 
 Освободит память по указателю ptr
 
 ** Пример использования: **
 
 1)Создаем класс:
 
 xtea3 *ptr_xtea_lib = new xtea3;

 2)Создадим ключ:
 
 uint32_t key[8] = {0x11, 0x55, 0xAA, 0x88, 0x12, 0x55, 0x77, 0x12};

 3)Зашифруем данные и получим указатель на зашифрованные данные:
 
 uint8_t *p_crypt_data = ptr_xtea_lib->data_crypt((uint8_t*)buffer_to_crypt, key, sizeof(buffer_to_crypt));
 
 Проверяем на нулл.

 4)Расшифруем данные и получим указатель на расшифрованные данные:
 
 uint8_t *p_decrypt_data = ptr_xtea_lib->data_decrypt((uint8_t*)p_crypt_data, key, ptr_xtea_lib->get_crypt_size());
 
 Проверяем на нулл.
 
 5)Работаем с расшифрованными/зашифрованными данными, через указатели p_crypt_data и p_decrypt_data.
 
 В конце освобождаем память, так:
 
 xtea3->free_ptr (p_crypt_data);
 
 xtea3->free_ptr (p_decrypt_data);
 
 **Можно шифровать как строки, так и любые бинарные данные (картинки, файлы и т.д.), В xtea3_lib.cpp ПРИМЕР КОДА ШИФРОВАНИЯ СТРОКИ И БИНАРНОГО ФАЙЛА, С ПОДРОБНЫМИ КОМЕНТАРИЯМИ. **
 
 ** В папке example, собранный пример.)**