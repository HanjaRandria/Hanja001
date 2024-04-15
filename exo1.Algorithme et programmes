def trouver_variables(expression):
    expression = expression.replace("and", "").replace("xor", "").replace("or", "").replace("not", "").replace(" ", "")
    variables = {char for char in expression if char.isalpha()}
    return sorted(variables)

def evaluer_expression(expression, valeurs_variables, variables):
    env = {var: val for var, val in zip(variables, valeurs_variables)}
    expression_python = expression.replace("and", "and").replace("or", "or").replace("not ", "not ").replace("xor", "^")
    try:
        return eval(expression_python, {"__builtins__": None}, env)
    except Exception as e:
        print(f"Erreur lors de l'évaluation de l'expression: {e}")
        return False

def forme_sop(variables, table_verite):
    termes = []
    for ligne in table_verite:
        if ligne[-1]:  # Si le résultat est vrai
            terme = []
            for var, val in zip(variables, ligne[:-1]):
                terme.append(f"{var}" if val else f"not {var}")
            termes.append('(' + ' and '.join(terme) + ')')
    return ' or '.join(termes) if termes else "False"

def forme_pos(variables, table_verite):
    termes = []
    for ligne in table_verite:
        if not ligne[-1]:  # Si le résultat est faux
            terme = []
            for var, val in zip(variables, ligne[:-1]):
                terme.append(f"{var}" if not val else f"not {var}")
            termes.append('(' + ' or '.join(terme) + ')')
    return ' and '.join(termes) if termes else "True"

def generer_table_verite_et_formes_canoniques(expression):
    variables = trouver_variables(expression)
    table_verite = []
    print(' | '.join(variables) + ' | Résultat')
    
    for i in range(2**len(variables)):
        valeurs_variables = [(i >> j) & 1 for j in range(len(variables)-1, -1, -1)]
        resultat = evaluer_expression(expression, valeurs_variables, variables)
        print(' | '.join(map(str, valeurs_variables)) + ' | ' + str(resultat))
        table_verite.append(valeurs_variables + [resultat])

    sop = forme_sop(variables, table_verite)
    pos = forme_pos(variables, table_verite)
    print("\nForme SOP (Somme de Produits) : ", sop)
    print("Forme POS (Produit de Sommes) : ", pos)

# Exemple d'utilisation
expression = input("Entrez la fonction booléenne (utilisez and, or, not, xor) : ")
generer_table_verite_et_formes_canoniques(expression)
