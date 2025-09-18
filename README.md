import { useState, useEffect } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { motion } from "framer-motion";

export default function LemosSite() {
  const [results, setResults] = useState([]);

  const speak = (text) => {
    if (typeof window !== "undefined" && "speechSynthesis" in window) {
      const utterance = new SpeechSynthesisUtterance(text);
      utterance.lang = "pt-BR";
      const voices = window.speechSynthesis.getVoices();
      const femaleVoice = voices.find(
        (v) => v.lang.startsWith("pt") && v.name.toLowerCase().includes("female")
      );
      if (femaleVoice) utterance.voice = femaleVoice;
      utterance.rate = 0.9;
      utterance.pitch = 1.1;
      window.speechSynthesis.speak(utterance);
    }
  };

  // Mensagem automÃ¡tica de boas-vindas ao entrar no site
  useEffect(() => {
    setTimeout(() => {
      speak("Bem-vindo ao buscador de voos Lemos");
    }, 1500);
  }, []);

  const handleSearch = () => {
    speak("Lemos");

    // SimulaÃ§Ã£o de busca de voos
    setResults([
      { id: 1, companhia: "Latam", horario: "08:00", duracao: "2h30", preco: "R$ 550" },
      { id: 2, companhia: "Gol", horario: "12:45", duracao: "2h20", preco: "R$ 480" },
      { id: 3, companhia: "Azul", horario: "19:10", duracao: "2h40", preco: "R$ 600" },
    ]);
  };

  const handleReserva = (companhia) => {
    speak(`Sua reserva na companhia ${companhia} foi iniciada`);
  };

  return (
    <div className="min-h-screen bg-green-950 text-white flex flex-col items-center">
      {/* Header */}
      <header className="w-full flex flex-col items-center py-6">
        <div className="w-full flex items-center justify-center relative">
          <div className="absolute w-full h-1 bg-orange-500 top-1/2 transform -translate-y-1/2"></div>
          <motion.h1
            className="relative text-5xl font-bold px-6 bg-green-950 text-orange-400"
            initial={{ opacity: 0, y: -50 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 1, ease: "easeOut" }}
            whileHover={{ scale: 1.1, textShadow: "0px 0px 8px rgba(255,165,0,0.8)" }}
          >
            Lemos
          </motion.h1>
        </div>
        <p className="mt-4 text-lg text-gray-300">Seu buscador de voos nacionais e internacionais</p>
      </header>

      {/* FormulÃ¡rio de busca */}
      <section className="bg-green-900 p-6 rounded-2xl shadow-lg w-full max-w-3xl mt-6">
        <div className="grid grid-cols-1 md:grid-cols-2 gap-4">
          <input type="text" placeholder="Origem" className="p-3 rounded-md text-black" />
          <input type="text" placeholder="Destino" className="p-3 rounded-md text-black" />
          <input type="date" className="p-3 rounded-md text-black" />
          <input type="date" className="p-3 rounded-md text-black" />
        </div>
        <div className="mt-4 flex justify-center">
          <motion.div
            whileHover={{ scale: 1.05, boxShadow: "0px 0px 12px rgba(255,165,0,0.6)" }}
            whileTap={{ scale: 0.95 }}
          >
            <Button onClick={handleSearch} className="bg-orange-500 hover:bg-orange-600 px-6 py-2 rounded-xl">
              Buscar Voos
            </Button>
          </motion.div>
        </div>
      </section>

      {/* Resultados da busca */}
      <section className="w-full max-w-4xl mt-10 space-y-4">
        {results.length > 0 && results.map((voo, index) => (
          <motion.div
            key={voo.id}
            initial={{ opacity: 0, y: 30 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.5, delay: index * 0.2 }}
          >
            <Card className="bg-green-800 text-white rounded-2xl shadow-lg">
              <CardContent className="flex justify-between items-center p-4">
                <div>
                  <h2 className="text-xl font-semibold">{voo.companhia}</h2>
                  <p>HorÃ¡rio: {voo.horario}</p>
                  <p>DuraÃ§Ã£o: {voo.duracao}</p>
                </div>
                <div className="text-right">
                  <p className="text-2xl font-bold text-orange-400">{voo.preco}</p>
                  <Button onClick={() => handleReserva(voo.companhia)} className="mt-2 bg-orange-500 hover:bg-orange-600">
                    Reservar
                  </Button>
                </div>
              </CardContent>
            </Card>
          </motion.div>
        ))}
      </section>

      {/* RodapÃ© */}
      <footer className="w-full text-center py-6 mt-10 border-t border-orange-500">
        <p className="text-gray-400">Â© 2025 Lemos - Todos os direitos reservados</p>
      </footer>
    </div>
  );
}
