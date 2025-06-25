#include <iostream>
#include <string>
#include <vector>
#include <random>
#include <algorithm>
#include <locale> // Для поддержки русского языка

// Функция для генерации случайного пароля
std::wstring generate_password(int length, bool use_digits, bool use_special_chars, bool avoid_similar) {
    std::wstring chars;

    // Базовые символы
    std::wstring lower_case = L"abcdefghijklmnopqrstuvwxyz";
    std::wstring upper_case = L"ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    std::wstring digits = L"0123456789";
    std::wstring special = L"!@#$%^&*()_+-=[]{}|;:,.<>?";

    // Добавляем символы в общий набор
    chars += lower_case + upper_case;
    if (use_digits) chars += digits;
    if (use_special_chars) chars += special;

    // Убираем похожие символы (если нужно)
    if (avoid_similar) {
        std::wstring similar = L"l1IoO0";
        for (wchar_t c : similar) {
            chars.erase(std::remove(chars.begin(), chars.end(), c), chars.end());
        }
    }

    // Проверка, что набор символов не пустой
    if (chars.empty()) {
        return L"Ошибка: Нет доступных символов для генерации пароля!";
    }

    // Генератор случайных чисел
    std::random_device rd;
    std::mt19937 gen(rd());
    std::uniform_int_distribution<> dist(0, chars.size() - 1);

    // Собираем пароль
    std::wstring password;
    for (int i = 0; i < length; ++i) {
        password += chars[dist(gen)];
    }

    return password;
}

int main() {
    // Устанавливаем локаль для поддержки русского языка
    setlocale(LC_ALL, "Russian");
    
    // Настройки генерации пароля
    int length = 12;
    bool use_digits = true;
    bool use_special_chars = true;
    bool avoid_similar = true;

    // Выводим меню на русском
    std::wcout << L"=== Генератор случайных паролей ===\n";
    std::wcout << L"Длина пароля: " << length << L" символов\n";
    std::wcout << L"Использовать цифры: " << (use_digits ? L"Да" : L"Нет") << L"\n";
    std::wcout << L"Использовать спецсимволы: " << (use_special_chars ? L"Да" : L"Нет") << L"\n";
    std::wcout << L"Исключать похожие символы: " << (avoid_similar ? L"Да" : L"Нет") << L"\n\n";

    // Генерируем и выводим пароль
    std::wstring password = generate_password(length, use_digits, use_special_chars, avoid_similar);
    std::wcout << L"Ваш пароль: " << password << L"\n\n";

    // :)
    std::wcout << L"----------------------------------------\n";
    std::wcout << L"Артем Андреевич, поставьте хороший автомат, я старался :)\n";
    std::wcout << L"----------------------------------------\n";

    return 0;
}
