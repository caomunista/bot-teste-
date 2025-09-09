        case "s": {
      // Verifica se a mensagem tem imagem
      if (msg.message?.imageMessage) {
        try {
          const buffer = await sock.downloadMediaMessage(msg); // baixa a imagem
          await sock.sendMessage(from, { 
            sticker: buffer 
          }, { quoted: msg });
        } catch (e) {
          await sock.sendMessage(from, { text: "❌ Erro ao criar figurinha." }, { quoted: msg });
        }
      } else {
        await sock.sendMessage(from, { text: "❌ Envie uma imagem com o comando !s" }, { quoted: msg });
      }
    }
    break;
