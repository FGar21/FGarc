# FGarc
Soy analista secundario de datos usando python, me interesa aprender técnicas para depurar los datos, el modelado y presentación de gráficos
import pandas as pd
import numpy as np
import matplotlib as plt
import matplotlib.pyplot as plt
import scipy.stats as stats
from scipy import stats
from scipy.stats import chi2_contingency

df = pd.DataFrame(
    [
        [176, 230],
        [21035, 21018],
        [23, 25],
        [7, 10],
        [23, 28],
        [29, 31],
        [28, 32],
        [23, 24],
        [53, 58],
        [18, 18],
        [26, 27],
        [47, 54]

    ],
    index=['1', '2', '3', '4', '5', '6', '7', '8', '9', '10', '11', '12'],
    columns=['Observado', 'Esperado']

)
# print(df)
#

# print(res)

gl = (df.shape[0]) - 1


def perform_chi_square_test(df, col1, col2):


    # Creating a contingency table
    contingency_table= pd.crosstab(df['Observado'], df['Esperado'])

    # Performing the Chi-Square Test
    chi2, p, dof, expected = chi2_contingency(contingency_table)

    # Interpreting the result
    significancia= p <= 0.07  # se establece en 27 % el nivel de significancia alfa
    return chi2, p, significancia

# Additional aspects to test in the Mathematics dataset
additional_aspects_to_test= {
    'Cantidad esperada vrs Cantidad Observada': ('Observado', 'Esperado')
    }

# Performing the additional tests for Mathematics dataset
additional_mat_chi_square_results = {aspect: perform_chi_square_test(df, *columns) for aspect, columns in additional_aspects_to_test.items()}
print(additional_mat_chi_square_results)

res= stats.chi2_contingency(df)
res2= chi2_contingency(df)

print(res2.statistic)
print(res2.pvalue)

chiCuadCalculada= res.statistic
valCritico = stats.chi2.ppf(1-0.93, gl)

ejeX= np.linspace(0, 30, 40)
ejeY= stats.chi2.pdf(ejeX, gl)

etiqueta1= 'Distribución de Chi-Cuadrado para: '
etiqueta2= 'grados de libertad'

plt.plot(ejeX, ejeY, label='Distribución Chi-Cuadrado para: '+str(gl)+' gl')
plt.axvline(chiCuadCalculada, color='black', linestyle='dashed', label='Valor de Chi-Cuadrado')
plt.axvline(valCritico, color='red', linestyle='dashed', label='Valor Crítico')
plt.fill_between(ejeX, ejeY, where=(ejeX > valCritico), color='red', alpha=0.07, label='Región de rechazo')
plt.title('Distribución de Chi-Cuadrada y Región Crítica para los proyectos SAF en hileras')
plt.xlabel('Valor calculado de Chi-Cuadrada')
plt.ylabel('Función de la Densidad probabilística')
plt.legend()
plt.show()
