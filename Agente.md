from openai import OpenAI

# Inicialización con OpenRouter
client = OpenAI(
    api_key="TU_OPENROUTER_API_KEY",
    base_url="https://openrouter.ai/api/v1",
)
MODELO = "deepseek/deepseek-v4.1-flash-1017"

# Prompt utilizando la base de conocimiento cargada
contexto_completo = "\n".join(documentos_docx[:15])  # Limitamos párrafos para evitar sobrecarga de tokens
prompt_sistema = f"""Actúa como el asistente virtual experto de Masas Delicias del Caribe.
Usa el siguiente contexto para responder a la consulta del usuario de forma amable y profesional.

Contexto:
{contexto_completo}
"""

pregunta_usuario = "¿Qué tipos de productos ofrece Masas Delicias del Caribe?"

response_llm = client.chat.completions.create(
    model=MODELO,
    messages=[
        {"role": "system", "content": prompt_sistema},
        {"role": "user", "content": pregunta_usuario},
    ],
    temperature=0.3,
)

print("--- Respuesta directa del LLM ---")
print(response_llm.choices[0].message.content)
