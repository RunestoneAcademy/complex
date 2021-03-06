..  Copyright (C)  Jan Pearce
    This work is licensed under the Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License. To view a copy of this license, visit http://creativecommons.org/licenses/by-nc-sa/4.0/.

.. _EOC_7:

The Simulation
--------------

The ``Simulation`` class for this chapter is based on the one in :ref:`Section 13.5 <EVO_5>`; the only differences are in ``__init__`` and ``step``.

Here’s the ``__init__`` method:

::

    class PDSimulation(Simulation):

        def __init__(self, tournament, agents):
            self.tournament = tournament
            self.agents = np.asarray(agents)
            self.instruments = []

A ``Simulation`` object contains a ``Tournament`` object, a sequence of ``agents``, and a sequence of ``Instrument`` objects (as in :ref:`Section 13.7 <Evo_7>`).

Here’s the ``step`` method:

::

    def step(self):
        self.tournament.melee(self.agents)
        Simulation.step(self)

This version of ``step`` USES ``Tournament.melee``, which sets the ``fitness`` attribute for each agent; then it calls the ``step`` method from the ``Simulation`` class, reproduced here:

::

    # class Simulation

        def step(self):
            n = len(self.agents)
            fits = self.get_fitnesses()

            # see who dies
            index_dead = self.choose_dead(fits)
            num_dead = len(index_dead)

            # replace the dead with copies of the living
            replacements = self.choose_replacements(num_dead, fits)
            self.agents[index_dead] = replacements

            # update any instruments
            self.update_instruments()

``Simulation.step`` collects the agents’ fitnesses in an array; then it calls ``choose_dead`` to decide which agents die, and ``choose_replacements`` to decide which agents reproduce.

The provided simulation includes differential survival, as in :ref:`Section 13.8 <EVO_8>`, but not differential reproduction. You can see the details in the notebook for this chapter. As one of the exercises, you will have a chance to explore the effect of differential reproduction.
