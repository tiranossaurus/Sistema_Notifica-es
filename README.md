interface CanalNotificacao {
    void enviar(String destinatario, String titulo, String conteudo);
}

class CanalEmail implements CanalNotificacao {
    @Override
    public void enviar(String destinatario, String titulo, String conteudo) {
        System.out.println("[E-MAIL -> " + destinatario + "] Assunto: " + titulo + " | " + conteudo);
    }
}

class CanalSms implements CanalNotificacao {
    @Override
    public void enviar(String destinatario, String titulo, String conteudo) {
        System.out.println("[SMS -> " + destinatario + "] " + titulo + ": " + conteudo);
    }
}

class CanalWhatsapp implements CanalNotificacao {
    @Override
    public void enviar(String destinatario, String titulo, String conteudo) {
        System.out.println("[WHATSAPP -> " + destinatario + "] *" + titulo + "*\n" + conteudo);
    }
}

abstract class Notificacao {
    protected CanalNotificacao canal;

    public Notificacao(CanalNotificacao canal) {
        this.canal = canal;
    }
    
    public abstract void notificar(String destinatario);
    
    public abstract void notificar(String destinatario, String mensagem);
}

class NotificacaoSimples extends Notificacao {
    public NotificacaoSimples(CanalNotificacao canal) {
        super(canal);
    }

    @Override
    public void notificar(String destinatario) {        
        notificar(destinatario, "Você recebeu uma nova atualização em sua conta.");
    }

    @Override
    public void notificar(String destinatario, String mensagem) {
        canal.enviar(destinatario, "Aviso", mensagem);
    }
}

class NotificacaoUrgente extends Notificacao {
    public NotificacaoUrgente(CanalNotificacao canal) {
        super(canal);
    }

    @Override
    public void notificar(String destinatario) {        
        notificar(destinatario, "Atenção necessária na sua conta.");
    }

    @Override
    public void notificar(String destinatario, String mensagem) {
        canal.enviar(destinatario, "URGENTE", "[AÇÃO IMEDIATA] " + mensagem);
    }
}

class NotificacaoCobranca extends Notificacao {
    public NotificacaoCobranca(CanalNotificacao canal) {
        super(canal);
    }

    @Override
    public void notificar(String destinatario) {        
        notificar(destinatario, "Sua fatura mensal já está fechada.");
    }

    @Override
    public void notificar(String destinatario, String mensagem) {
        canal.enviar(destinatario, "Cobrança", "Detalhes da fatura: " + mensagem);
    }
}

public class SistemaNotificacoes {
    public static void main(String[] args) {
        CanalNotificacao email = new CanalEmail();
        CanalNotificacao sms = new CanalSms();
        CanalNotificacao whatsapp = new CanalWhatsapp();
        
        Notificacao avisoPadrao = new NotificacaoSimples(email);
        avisoPadrao.notificar("ana@exemplo.com");

        Notificacao cobrancaPadrao = new NotificacaoCobranca(whatsapp);
        cobrancaPadrao.notificar("+5583988880000");

        System.out.println("--------------------------------------------------");
        
        Notificacao alertaCustomizado = new NotificacaoUrgente(sms);
        alertaCustomizado.notificar("+5583999990000", "O servidor de banco de dados entrou em curto-circuito.");

        Notificacao cobrancaCustomizada = new NotificacaoCobranca(email);
        cobrancaCustomizada.notificar("carlos@exemplo.com", "Parcela em até 15 vezes no cartão de crédito.");
    }
}
