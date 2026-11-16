LAB 1 : WRITE A C PROGRAM THAT CONTAINS A STRING (CHAR POINTER) 
\HELLO WORLD. THE PROGRAM SHOULD XOR EACH CHARACTER IN THIS 
STRING WITH ZERO AND DISPLAY THE RESULT
#include <stdio.h>
int main() {
    char *str = "Hello World"; 
    int i = 0; 
    printf("Original string: %s\n", str); 
    printf("After XOR with 0: ");
   while(str[i]!= '\0') { 
        char result = str[i] ^ 0; 
        printf("%c", result); 
        i++; 
    }
    printf("\n"); 
    return 0; 
}


lab 2: WRITE A C PROGRAM THAT CONTAINS A STRING (CHAR POINTER) 
\HELLO WORLD. THE PROGRAM SHOULD AND & XOR EACH CHARACTER IN 
THIS STRING WITH 127 AND DISPLAY THE RESULT
#include <stdio.h>
int main() {
    char *str = "Hello World"; 
    printf("Original string: %s\n", str);
    // Using AND with 127
    printf("After AND with 127:\n");
    for (int i = 0; str[i]!= '\0'; i++) {
        char result = str[i] & 127;
        printf("%c", result);
    }
    printf("\n\n");
    // Using XOR with 127
    printf("After XOR with 127:\n");
    for (int i = 0; str[i]!= '\0'; i++) {
        char result = str[i] ^ 127;
        printf("%c", result);
    }
    printf("\n");
    return 0;
}



lab 3 (a): WRITE A JAVA PROGRAM TO PERFORM ENCRYPTION AND 
DECRYPTION USING THE CAESAR CIPHER
import java.util.Scanner;
public class SimpleCaesar {
    
    // Shift one character
    static char shiftChar(char ch, int shift) {
        if (ch >= 'A' && ch <= 'Z') {
            return (char) ('A' + (ch - 'A' + shift) % 26);
        }
        if (ch >= 'a' && ch <= 'z') {
            return (char) ('a' + (ch - 'a' + shift) % 26);
        }
        return ch; // don't change spaces, numbers, symbols
    }
    // Encrypt
    static String encrypt(String text, int shift) {
        StringBuilder sb = new StringBuilder();
        for (char ch : text.toCharArray()) {
            sb.append(shiftChar(ch, shift));
        }
        return sb.toString();
    }
    // Decrypt
    static String decrypt(String text, int shift) {
        return encrypt(text, 26 - shift); // just shift backward
    }
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter text: ");
        String text = sc.nextLine();
        System.out.print("Enter shift: ");
        int shift = sc.nextInt();
        System.out.println("Encrypted: " + encrypt(text, shift));
        System.out.println("Decrypted: " + decrypt(encrypt(text, shift), shift));
        sc.close();
    }
}


LAB 4: WRITE A JAVA PROGRAM TO PERFORM ENCRYPTION AND DECRYPTION 
USING RAIL FENCE TRANSPOSITION TECHNIQUE
import java.util.*;
class RailFenceCipher {
    static String encrypt(String t, int r){
        StringBuilder[] a = new StringBuilder[r];
        for(int i=0;i<r;i++) a[i]=new StringBuilder();
        int row=0,d=1;
        for(char c:t.toCharArray()){
            a[row].append(c);
            if(row==0) d=1; else if(row==r-1) d=-1;
            row+=d;
        }
        StringBuilder s=new StringBuilder();
        for(var x:a) s.append(x);
        return s.toString();
    }
    static String decrypt(String c, int r){
        int n=c.length(); char[][] f=new char[r][n];
        int row=0,d=1;
        for(int i=0;i<n;i++)
        { f[row][i]='*';   if(row==0) d=1;   else if(row==r-1)   d=-1;   row+=d; }
        int k=0;
        for(int i=0;i<r;i++) for(int j=0;j<n;j++)  if(f[i][j]=='*') f[i][j]=c.charAt(k++);
        StringBuilder s=new StringBuilder();  row=0; d=1;
        for(int i=0;i<n;i++)
       { s.append(f[row][i]); if(row==0) d=1; else if(row==r-1) d=-1; row+=d; }
        return s.toString();
    }
    public static void main(String[] a){
        Scanner sc=new Scanner(System.in);
        System.out.print("Text: "); String t=sc.nextLine();
        System.out.print("Rails: "); int r=sc.nextInt();
        String e=encrypt(t,r);
        System.out.println("Encrypted: "+e);
        System.out.println("Decrypted: "+decrypt(e,r));
    }
}


LAB 6: WRITE A JAVA PROGRAM TO IMPLEMENT DES ALGORITHM LOGIC
import javax.crypto.Cipher;
import javax.crypto.SecretKey;
import javax.crypto.SecretKeyFactory;
import javax.crypto.spec.DESKeySpec;
import java.util.Base64;
import java.util.Scanner;
public class DESWithUserKey {
    public static void main(String[] args) throws Exception {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter Plaintext: ");
        String plainText = sc.nextLine();
        System.out.print("Enter Key (must be 8 chars e.g. 12345678): ");
        String keyInput = sc.nextLine();
        // 1. Make key from user input string
        DESKeySpec desKeySpec = new DESKeySpec(keyInput.getBytes());
        SecretKeyFactory keyFactory = SecretKeyFactory.getInstance("DES");
        SecretKey secretKey = keyFactory.generateSecret(desKeySpec);
        // 2. Encrypt
        Cipher cipher = Cipher.getInstance("DES/ECB/PKCS5Padding");
        cipher.init(Cipher.ENCRYPT_MODE, secretKey);
        byte[] encryptedBytes = cipher.doFinal(plainText.getBytes());
        String encryptedText = 
Base64.getEncoder().encodeToString(encryptedBytes);
        System.out.println("Encrypted: " + encryptedText);
        // 3. Decrypt
        cipher.init(Cipher.DECRYPT_MODE, secretKey);
        byte[] decryptedBytes = 
cipher.doFinal(Base64.getDecoder().decode(encryptedText));
        String decryptedText = new String(decryptedBytes);
        System.out.println("Decrypted: " + decryptedText);
    }
}



LAB 7: WRITE A JAVA PROGRAM TO IMPLEMENT AES ALGORITHM LOGIC
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;
import java.util.Scanner;
public class AES {
    public static void main(String[] args) throws Exception {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter 16 char Key: ");
        String key = sc.nextLine();
        System.out.print("Enter Plain Text: ");
        String plain = sc.nextLine();
        // Encrypt
        SecretKeySpec secKey = new SecretKeySpec(key.getBytes(), "AES");
        Cipher cipher = Cipher.getInstance("AES");
        
        cipher.init(Cipher.ENCRYPT_MODE, secKey);
        String encrypted = 
Base64.getEncoder().encodeToString(cipher.doFinal(plain.getBytes()));
        // Decrypt
        cipher.init(Cipher.DECRYPT_MODE, secKey);
        byte[] decoded = Base64.getDecoder().decode(encrypted);
        String decrypted = new String(cipher.doFinal(decoded));
        System.out.println("\nEncrypted : " + encrypted);
        System.out.println("Decrypted : " + decrypted);
        
        sc.close();
    }
}



LAB 8: WRITE A JAVA PROGRAM TO IMPLEMENT TRIPLE DES ALGORITHM 
LOGIC
import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;
import java.util.Scanner;
public class TripleDES {
    public static void main(String[] args) throws Exception {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter 24 char Key: ");
        String key = sc.nextLine();
        System.out.print("Enter Plain Text: ");
        String plain = sc.nextLine();
        // Triple DES needs 24 byte key
        SecretKeySpec secKey = new SecretKeySpec(key.getBytes(), "DESede");
        Cipher cipher = Cipher.getInstance("DESede"); // 
DESede/ECB/PKCS5Padding
        // Encrypt
        cipher.init(Cipher.ENCRYPT_MODE, secKey);
        String encrypted = 
Base64.getEncoder().encodeToString(cipher.doFinal(plain.getBytes()));
        // Decrypt
        cipher.init(Cipher.DECRYPT_MODE, secKey);
        String decrypted = new 
String(cipher.doFinal(Base64.getDecoder().decode(encrypted)));
        System.out.println("\nEncrypted : " + encrypted);
        System.out.println("Decrypted : " + decrypted);
        
        sc.close();
    }
}



LAB 9: WRITE A JAVA PROGRAM TO IMPLEMENT RC4 ALGORITHM LOGIC, 
ENCRYPT “HELLO WORLD”
import javax.crypto.Cipher;
import javax.crypto.KeyGenerator;
import javax.crypto.SecretKey;
import java.util.Base64;
public class RC4 {
    public static void main(String[] args) throws Exception {
        String plain = "Hello world";
        
        KeyGenerator kg = KeyGenerator.getInstance("ARCFOUR");
        kg.init(128); //40 to 2048
        SecretKey key = kg.generateKey();
        Cipher c = Cipher.getInstance("ARCFOUR");
        
        c.init(Cipher.ENCRYPT_MODE, key);
        String enc = 
Base64.getEncoder().encodeToString(c.doFinal(plain.getBytes()));
        c.init(Cipher.DECRYPT_MODE, key);
        String dec = new String(c.doFinal(Base64.getDecoder().decode(enc)));
        System.out.println("Key: " + 
Base64.getEncoder().encodeToString(key.getEncoded()));
        System.out.println("Plain: " + plain);
        System.out.println("Enc: " + enc);
        System.out.println("Dec: " + dec);
    }
}



LAB 10: WRITE A JAVA PROGRAM TO IMPLEMENT RSA ALGORITHM LOGIC
import javax.crypto.Cipher;
import java.security.KeyPair;
import java.security.KeyPairGenerator;
import java.util.Base64;
import java.util.Scanner;
public class RSA {
    public static void main(String[] args) throws Exception {
        Scanner sc = new Scanner(System.in);
        System.out.print("Enter Plain Text: ");
        String plain = sc.nextLine();
// 1. Generate Key Pair -1024, 2048, 3072 or 4096 bits.
        KeyPairGenerator keyGen = KeyPairGenerator.getInstance("RSA");
        keyGen.initialize(2048);
        KeyPair pair = keyGen.generateKeyPair();
 // 2. Encrypt with Public Key
        Cipher cipher = Cipher.getInstance("RSA");
        cipher.init(Cipher.ENCRYPT_MODE, pair.getPublic());
        String encrypted = 
Base64.getEncoder().encodeToString(cipher.doFinal(plain.getBytes()));
 // 3. Decrypt with Private Key
        cipher.init(Cipher.DECRYPT_MODE, pair.getPrivate());
        String decrypted = new 
String(cipher.doFinal(Base64.getDecoder().decode(encrypted)));
        System.out.println("\nEncrypted : " + encrypted);
        System.out.println("Decrypted : " + decrypted);
        
        sc.close();
    }
}


