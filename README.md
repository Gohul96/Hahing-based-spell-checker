import java.util.Scanner;
import java.util.Hashtable;

public class SpellChecker {
    public static void main(String[] args) {
        Hashtable<String, Integer> dictionary = new Hashtable<>();
        
        // Load the dictionary with correctly spelled words
        loadDictionary(dictionary);
        
        // Create a scanner to read user input
        Scanner scanner = new Scanner(System.in);
        
        while (true) {
            System.out.print("Enter a word (q to quit): ");
            String input = scanner.nextLine().toLowerCase();
            
            if (input.equals("q")) {
                break;
            }
            
            if (!dictionary.containsKey(input)) {
                System.out.println("Misspelled word: " + input);
                suggestCorrections(dictionary, input);
            } else {
                System.out.println("Correctly spelled word.");
            }
        }
        
        scanner.close();
    }
    
    // Load the dictionary with correctly spelled words
    private static void loadDictionary(Hashtable<String, Integer> dictionary) {
        // Add correctly spelled words to the dictionary
        dictionary.put("apple", 1);
        dictionary.put("banana", 1);
        dictionary.put("cherry", 1);
        dictionary.put("date", 1);
        dictionary.put("fig", 1);
        dictionary.put("grape", 1);
        dictionary.put("kiwi", 1);
        dictionary.put("lemon", 1);
        dictionary.put("lime", 1);
        dictionary.put("mango", 1);
        dictionary.put("orange", 1);
        dictionary.put("pear", 1);
        dictionary.put("plum", 1);
        dictionary.put("strawberry", 1);
        dictionary.put("watermelon", 1);
    }
    
    // Suggest corrections for a misspelled word
    private static void suggestCorrections(Hashtable<String, Integer> dictionary, String word) {
        System.out.print("Suggestions: ");
        
        for (String key : dictionary.keySet()) {
            if (levenshteinDistance(key, word) <= 2) {
                System.out.print(key + " ");
            }
        }
        
        System.out.println();
    }
    
    // Calculate the Levenshtein distance between two words
    private static int levenshteinDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        int[][] dp = new int[m + 1][n + 1];
        
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                int cost = (word1.charAt(i - 1) == word2.charAt(j - 1)) ? 0 : 1;
                dp[i][j] = Math.min(Math.min(dp[i - 1][j] + 1, dp[i][j - 1] + 1), dp[i - 1][j - 1] + cost);
            }
        }
        
        return dp[m][n];
    }
}
