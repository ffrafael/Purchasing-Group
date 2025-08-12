# Purchasing-Group
Basic Form to Capture Leads

# Estrutura do banco de dados para conversões parciais
CREATE TABLE leads (
    id VARCHAR(50) PRIMARY KEY,
    nome VARCHAR(255),
    email VARCHAR(255),
    whatsapp VARCHAR(20),
    cursos TEXT,
    completed BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT NOW(),
    updated_at TIMESTAMP DEFAULT NOW()
);

-- Índices para otimização
CREATE INDEX idx_leads_email ON leads(email);
CREATE INDEX idx_leads_completed ON leads(completed);
CREATE INDEX idx_leads_created_at ON leads(created_at);

# Implementação Backend (Node.js + Supabase)
// Salvar dados parciais
async function savePartialLead(leadData) {
    const { data, error } = await supabase
        .from('leads')
        .upsert({
            id: leadData.id,
            [leadData.field]: leadData.value,
            updated_at: new Date().toISOString()
        });
    return { data, error };
}

// Completar lead
async function completeLead(leadData) {
    const { data, error } = await supabase
        .from('leads')
        .update({
            ...leadData,
            completed: true,
            updated_at: new Date().toISOString()
        })
        .eq('id', leadData.id);
    return { data, error };
}
