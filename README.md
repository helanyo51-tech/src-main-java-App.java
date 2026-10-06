import com.google.gson.*;
import java.io.FileReader;
import java.time.*;
import java.time.format.TextStyle;
import java.util.Locale;

public class App {
    public static void main(String[] args) throws Exception {
        String dia = LocalDate.now(ZoneId.of("America/Fortaleza"))
            .getDayOfWeek().getDisplayName(TextStyle.FULL, new Locale("pt", "BR"))
            .toLowerCase().replace("-feira", "").replace("á", "a");

        JsonObject ficha = JsonParser.parseReader(new FileReader("treinos.json")).getAsJsonObject();

        if (!ficha.has(dia)) {
            System.out.println("Hoje é dia de descanso! 😴");
            return;
        }
        JsonObject treino = ficha.getAsJsonObject(dia);
        System.out.println("🏋️ " + treino.get("nome").getAsString());
        for (JsonElement e : treino.getAsJsonArray("exercicios")) {
            JsonObject ex = e.getAsJsonObject();
            System.out.println("- " + ex.get("nome").getAsString() + ": "
                + ex.get("series").getAsInt() + "x" + ex.get("reps").getAsInt());
        }
    }
}
